# GC Analytics Migration Configuration Summary

## Overview
This document describes the generated JSON configuration for the GC Analytics T1 (Incremental) Migration.

## Configuration File
- **File**: `gc_analytics_migration_config.json`
- **Domain**: `gc_analytics`
- **Total Tables**: 17

## Configured Tables

The following tables have been configured with complete migration settings:

1. **fd_chart_ciq** - FD Chart CIQ data
2. **device_shared_networks** - Device shared networks information
3. **mobile_token** - Mobile token data
4. **sso_user** - SSO user information
5. **fp_cache** - FP cache data
6. **mobile_device** - Mobile device information
7. **mdi_user** - MDI user data
8. **persona** - Persona information
9. **persona_bedtime_multiple** - Persona bedtime multiple settings
10. **persona_stations** - Persona stations data
11. **mdi_router** - MDI router information
12. **cafii_latency_test_resp** - CAFII latency test responses
13. **cafii_speed_test_req** - CAFII speed test requests
14. **cafii_speed_test_resp** - CAFII speed test responses
15. **cloud_speed_test** - Cloud speed test data
16. **wifi_pool_primary_networks** - WiFi pool primary networks
17. **cafii_latency_test_req** - CAFII latency test requests

## Configuration Structure

Each table includes the following configuration sections:

### Core Configuration
- **source_system**: "map"
- **incremental_sort_column**: "created"
- **merge_key**: "correlation_id" for cafii/speed_test tables, "id" for others
- **org_inclusion_exclusion_field**: "org_id"
- **is_this_incremental_table**: true

### Table Metadata
- **category**: "user_config_small"
- **estimated_size_mb**: 50
- **supports_deletes**: false
- **org_specific**: true
- **auto_discover_schema**: true

### Migration Configuration
- **t0_strategy**: "presto_direct"
- **t1_strategy**: "presto_incremental"
- **load_strategy**: "upsert"
- **batch_size**: 10000
- **incremental_timestamp_check**: true

### Extraction Configuration
- **Source**: map_cassandra.map_stage schema
- **Chunking Strategy**: org_based
- **Chunk Size**: 10000
- **Excluded SPIDs**: "470053", "12896222"
- **Incremental Column**: "created"

### Validation Configuration
- **Row Count Validation**: Enabled (0.5% tolerance)
- **Schema Validation**: Enabled (auto-discover)
- **Data Freshness Validation**: Enabled (using "created" column)

### DBT Configuration
- **Target Schema**: core
- **DBT Render Modules**: org_exclusion_clause
- **Materialization**: Incremental with upsert strategy

### SPID Resolution
- **Has Direct Org Field**: true
- **Direct Org Field**: "org_id"
- **Requires Join for SPID**: false

## Usage

This configuration file can be used to:
1. Configure migration pipelines for the listed tables
2. Set up incremental data extraction from Cassandra to AlloyDB
3. Define validation rules and data quality checks
4. Configure DBT models for data transformation

## Notes

- All tables are configured for incremental migration (T1 strategy)
- Organization-based filtering is enabled for all tables
- SPIDs 470053 and 12896222 are excluded from all extractions
- All tables use the "created" timestamp for incremental loading
- CAFII and speed test tables use "correlation_id" as the merge key
- Other tables use "id" as the merge key
