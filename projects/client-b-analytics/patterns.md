# Client B - Analytics Dashboard - Patterns

**Project:** DataFlow Analytics Platform
**Last Updated:** 2025-11-10

Patterns discovered during data pipeline and analytics dashboard development.

---

## 🎯 HIGH CONFIDENCE PATTERNS (5+ applications)

### Pattern: Incremental Data Sync with Watermark

**Context:** Syncing marketing data from APIs (Google Ads, Facebook, LinkedIn)

**Problem:** Full data sync takes hours and wastes API quota. Need to sync only new/changed data.

**Solution:**
```python
# Store last sync timestamp per connector
class IncrementalSync:
    def __init__(self, connector_id: str):
        self.connector_id = connector_id
        self.watermark_key = f"sync:watermark:{connector_id}"

    async def sync(self):
        # 1. Get last sync timestamp (watermark)
        last_sync = await redis.get(self.watermark_key)
        if not last_sync:
            last_sync = datetime.now() - timedelta(days=90)  # Initial: 90 days

        # 2. Fetch only data updated since watermark
        new_data = await self.fetch_data_since(last_sync)

        # 3. Save data to database
        await self.save_to_db(new_data)

        # 4. Update watermark to current time
        await redis.set(self.watermark_key, datetime.now().isoformat())

        return len(new_data)

    async def fetch_data_since(self, since: datetime):
        # Example: Google Ads API
        return await google_ads_client.get_campaigns(
            date_range={
                'start_date': since.isoformat(),
                'end_date': datetime.now().isoformat()
            }
        )
```

**When to Apply:**
- ✅ Marketing API integrations (Ads platforms)
- ✅ CRM sync (HubSpot, Salesforce)
- ✅ Any large dataset that grows over time

**Benefits:**
- Reduces API calls by 90-95%
- Sync time: 2 hours → 5 minutes
- Stays within API rate limits

**Confidence:** HIGH (used across 5 connectors: Google, Facebook, LinkedIn, HubSpot, Stripe)

---

### Pattern: TimescaleDB Continuous Aggregates for Analytics

**Context:** Dashboard queries on raw time-series data are slow (6-10 seconds)

**Problem:** Aggregating millions of rows every query is expensive

**Solution:**
```sql
-- Create continuous aggregate for daily metrics
CREATE MATERIALIZED VIEW daily_campaign_metrics
WITH (timescaledb.continuous) AS
SELECT
    time_bucket('1 day', date) AS day,
    campaign_id,
    SUM(impressions) AS total_impressions,
    SUM(clicks) AS total_clicks,
    SUM(spend) AS total_spend,
    AVG(ctr) AS avg_ctr
FROM campaign_performance
GROUP BY day, campaign_id;

-- Refresh policy: Update aggregate every 15 minutes
SELECT add_continuous_aggregate_policy('daily_campaign_metrics',
    start_offset => INTERVAL '3 days',
    end_offset => INTERVAL '1 hour',
    schedule_interval => INTERVAL '15 minutes');

-- Query the aggregate instead of raw data
-- BEFORE: 8.2 seconds
SELECT * FROM campaign_performance
WHERE date >= NOW() - INTERVAL '30 days'
GROUP BY campaign_id;

-- AFTER: 0.3 seconds
SELECT * FROM daily_campaign_metrics
WHERE day >= NOW() - INTERVAL '30 days';
```

**When to Apply:**
- ✅ Analytics dashboards
- ✅ Time-series data with aggregations (SUM, AVG, COUNT)
- ✅ Queries on fixed time ranges (daily, hourly, weekly)

**Results:**
- Query time: 8.2s → 0.3s (27x faster)
- No code changes needed (just query different table)
- Auto-refreshes on new data

**Confidence:** HIGH (implemented for 5 metric types, all faster)

---

### Pattern: API Rate Limit Handling with Circuit Breaker

**Context:** External APIs (Facebook, LinkedIn) have strict rate limits

**Problem:** Hitting rate limit causes cascading failures, long retry delays

**Solution:**
```python
from circuitbreaker import circuit
import tenacity

class RateLimitedAPIClient:
    def __init__(self, api_client, rate_limit: int):
        self.api_client = api_client
        self.rate_limit = rate_limit  # requests per hour
        self.request_count = 0
        self.window_start = datetime.now()

    @circuit(failure_threshold=5, recovery_timeout=300)
    @tenacity.retry(
        wait=tenacity.wait_exponential(min=1, max=60),
        stop=tenacity.stop_after_attempt(5),
        retry=tenacity.retry_if_exception_type(RateLimitError)
    )
    async def request(self, endpoint: str, params: dict):
        # 1. Check if we're within rate limit
        if self._is_rate_limited():
            sleep_time = self._time_until_reset()
            logger.warning(f"Rate limit reached. Sleeping {sleep_time}s")
            await asyncio.sleep(sleep_time)
            self._reset_window()

        # 2. Make request
        try:
            response = await self.api_client.get(endpoint, params=params)
            self.request_count += 1
            return response
        except RateLimitError as e:
            # Circuit breaker will trip after 5 failures
            logger.error(f"Rate limit error: {e}")
            raise

    def _is_rate_limited(self) -> bool:
        # Check if we've hit rate limit in current window
        if datetime.now() - self.window_start > timedelta(hours=1):
            self._reset_window()

        return self.request_count >= self.rate_limit

    def _time_until_reset(self) -> int:
        elapsed = (datetime.now() - self.window_start).total_seconds()
        return max(0, 3600 - elapsed)  # 3600s = 1 hour

    def _reset_window(self):
        self.window_start = datetime.now()
        self.request_count = 0
```

**When to Apply:**
- ✅ External API integrations with rate limits
- ✅ Payment gateways (Stripe, PayPal)
- ✅ Social media APIs (LinkedIn, Twitter)

**Benefits:**
- Prevents cascading failures (circuit breaker)
- Automatic retry with exponential backoff
- Stays within rate limits

**Confidence:** HIGH (solved Facebook rate limit issue, now using for all APIs)

---

## 🔄 MEDIUM CONFIDENCE PATTERNS (2-4 applications)

### Pattern: Batch API Requests for Efficiency

**Context:** Facebook Ads API charges per request (200 requests/hour limit)

**Problem:** Fetching 100 campaigns = 100 API requests (hits limit fast)

**Solution:**
```python
# Facebook Batch API
# BEFORE: 100 requests (one per campaign)
campaigns = []
for campaign_id in campaign_ids:
    campaign = await fb_client.get_campaign(campaign_id)
    campaigns.append(campaign)

# AFTER: 10 requests (10 campaigns per batch)
import aiohttp

async def batch_get_campaigns(campaign_ids: list[str]) -> list[dict]:
    batches = [campaign_ids[i:i+10] for i in range(0, len(campaign_ids), 10)]

    all_campaigns = []
    for batch in batches:
        # Single API request with batch parameter
        batch_request = {
            'batch': [
                {'method': 'GET', 'relative_url': f'{id}?fields=name,status,spend'}
                for id in batch
            ]
        }

        response = await fb_client.post('/', json=batch_request)
        all_campaigns.extend(response['data'])

    return all_campaigns
```

**Results:**
- API calls: 100 → 10 (90% reduction)
- Sync time: 45 min → 5 min
- Rate limit issues: Resolved

**When to Apply:**
- ✅ Facebook Marketing API
- ✅ LinkedIn Ads API
- ✅ Any API supporting batch requests

**Confidence:** MEDIUM (used for Facebook and LinkedIn, need to validate with more APIs)

---

### Pattern: FastAPI WebSocket Connection Pooling

**Context:** Real-time dashboard updates via WebSocket

**Problem:** Each user = 1 WebSocket connection. 1000 users = 1000 connections per server instance (too many)

**Solution:**
```python
from fastapi import WebSocket, WebSocketDisconnect
import asyncio

class ConnectionPool:
    def __init__(self, max_connections: int = 1000):
        self.active_connections: dict[str, WebSocket] = {}
        self.max_connections = max_connections

    async def connect(self, client_id: str, websocket: WebSocket):
        if len(self.active_connections) >= self.max_connections:
            await websocket.close(code=1008, reason="Max connections reached")
            return False

        await websocket.accept()
        self.active_connections[client_id] = websocket
        return True

    def disconnect(self, client_id: str):
        self.active_connections.pop(client_id, None)

    async def broadcast(self, message: dict):
        # Send to all connected clients
        disconnected = []
        for client_id, websocket in self.active_connections.items():
            try:
                await websocket.send_json(message)
            except WebSocketDisconnect:
                disconnected.append(client_id)

        # Clean up disconnected clients
        for client_id in disconnected:
            self.disconnect(client_id)

pool = ConnectionPool(max_connections=1000)

@app.websocket("/ws/{client_id}")
async def websocket_endpoint(websocket: WebSocket, client_id: str):
    if not await pool.connect(client_id, websocket):
        return

    try:
        while True:
            # Keep connection alive
            await websocket.receive_text()
    except WebSocketDisconnect:
        pool.disconnect(client_id)

# Broadcast updates when new data arrives
@app.post("/webhooks/data-sync-complete")
async def on_data_sync(sync_event: dict):
    await pool.broadcast({
        'type': 'data_updated',
        'timestamp': datetime.now().isoformat()
    })
```

**When to Apply:**
- ✅ Real-time dashboards
- ✅ Collaborative tools (live cursors, comments)
- ✅ Notification systems

**Benefits:**
- Limit max connections per instance
- Graceful handling when limit reached
- Efficient broadcast to all clients

**Confidence:** MEDIUM (works in testing, need production validation with 1000+ users)

---

### Pattern: dbt Data Transformation Pipeline

**Context:** Raw data from Airbyte needs transformation (cleaning, aggregation, business logic)

**Problem:** Hardcoding transformations in Python is messy, hard to test, no version control for SQL logic

**Solution:**
```sql
-- dbt model: models/staging/stg_google_ads__campaigns.sql
-- Clean and standardize Google Ads data

{{ config(materialized='view') }}

WITH source AS (
    SELECT * FROM {{ source('airbyte', 'google_ads_campaigns') }}
),

cleaned AS (
    SELECT
        id AS campaign_id,
        name AS campaign_name,
        status,
        CAST(daily_budget AS DECIMAL(10,2)) AS daily_budget_usd,
        DATE(created_at) AS created_date,
        DATE(updated_at) AS updated_date
    FROM source
    WHERE status != 'REMOVED'  -- Filter deleted campaigns
)

SELECT * FROM cleaned
```

```sql
-- dbt model: models/marts/fct_campaign_performance.sql
-- Business logic: Calculate ROI, CTR, CPC

{{ config(materialized='table') }}

WITH campaigns AS (
    SELECT * FROM {{ ref('stg_google_ads__campaigns') }}
),

performance AS (
    SELECT * FROM {{ ref('stg_google_ads__performance') }}
),

joined AS (
    SELECT
        c.campaign_id,
        c.campaign_name,
        p.date,
        p.impressions,
        p.clicks,
        p.spend,
        p.conversions,
        p.conversion_value,

        -- Calculated metrics
        CASE
            WHEN p.impressions > 0
            THEN (p.clicks::DECIMAL / p.impressions) * 100
            ELSE 0
        END AS ctr,

        CASE
            WHEN p.clicks > 0
            THEN p.spend / p.clicks
            ELSE 0
        END AS cpc,

        CASE
            WHEN p.spend > 0
            THEN ((p.conversion_value - p.spend) / p.spend) * 100
            ELSE 0
        END AS roi
    FROM campaigns c
    INNER JOIN performance p ON c.campaign_id = p.campaign_id
)

SELECT * FROM joined
```

**dbt Benefits:**
- Version-controlled SQL transformations
- Dependency management (refs)
- Testing framework (schema tests, data tests)
- Documentation auto-generated
- Incremental builds (only transform new data)

**When to Apply:**
- ✅ Data pipelines with complex transformations
- ✅ Analytics dashboards (pre-compute metrics)
- ✅ Data warehouses

**Confidence:** MEDIUM (powerful but learning curve steep, still optimizing our models)

---

## 🔬 LOW CONFIDENCE PATTERNS (1 application)

### Pattern: Data Quality Validation with Great Expectations

**Context:** Need to validate data quality (row counts, metric accuracy)

**Problem:** Manual data validation is time-consuming and error-prone

**Solution:**
```python
import great_expectations as ge

# Define expectations
def validate_campaign_data(df: pd.DataFrame):
    ge_df = ge.from_pandas(df)

    # Row count expectations
    ge_df.expect_table_row_count_to_be_between(min_value=1, max_value=10000)

    # Column expectations
    ge_df.expect_column_values_to_not_be_null('campaign_id')
    ge_df.expect_column_values_to_be_unique('campaign_id')

    # Metric validation
    ge_df.expect_column_values_to_be_between('spend', min_value=0, max_value=100000)
    ge_df.expect_column_values_to_be_between('ctr', min_value=0, max_value=100)

    # Run validation
    results = ge_df.validate()

    if not results['success']:
        send_alert_to_slack(results)

    return results
```

**When to Apply:**
- ✅ Data pipeline validation
- ✅ Data quality monitoring
- ✅ ML model input validation

**Considerations:**
- Setup overhead (expectations config)
- Learning curve (GE concepts)
- May be overkill for simple pipelines

**Confidence:** LOW (just implemented, need to see how it performs in production)

---

## 📚 Patterns to Extract (Next /learn Session)

**Potential Patterns Noticed:**
1. **Airbyte Connector Configuration** - YAML structure, secrets management, normalization settings
2. **Airflow DAG Best Practices** - Task dependencies, retries, SLAs, alerting
3. **FastAPI Async Database Queries** - Connection pooling, query optimization with asyncio
4. **TimescaleDB Compression Policies** - When to compress, how much storage saved
5. **Redis Caching Strategy** - TTL selection, cache invalidation, memory management

**Next Steps:** Run `/learn` after sprint ends (Nov 23) to extract these patterns

---

## 🔗 Cross-References

**Related Semantic Memory:**
- `memory/semantic/tech/data-pipelines.md` - Airbyte, dbt, Airflow patterns
- `memory/semantic/tech/timescaledb-optimization.md` - TimescaleDB-specific patterns
- `memory/semantic/tech/api-integrations.md` - Rate limiting, batch requests

**Related Episodic Memory:**
- `memory/episodic/2025-11-09-facebook-rate-limit-debugging.md` - 8-hour debugging session
- `memory/episodic/2025-11-07-timescaledb-continuous-aggregates.md` - Research and implementation

**Related Projects:**
- `projects/internal-saas/` - Also uses FastAPI, can reuse WebSocket patterns
- `projects/client-a-ecommerce/` - Shares PostgreSQL optimization patterns

---

**Pattern Extraction Stats:**
- Total Patterns: 8
- HIGH Confidence: 3 (reusable immediately)
- MEDIUM Confidence: 4 (need production validation)
- LOW Confidence: 1 (experimental)
- Potential Patterns: 5 (need extraction)

**Next Review:** After sprint ends (Nov 23, 2025)
