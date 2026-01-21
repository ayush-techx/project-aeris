# Requirements Document

## Introduction

AERIS (AI-Verified Pollution Compliance Network) is an AIoT platform that incentivizes rural industries, specifically brick kilns, to reduce pollution through a dual approach: fraud detection mechanisms ("The Stick") combined with carbon credit rewards ("The Carrot"). The system uses edge telemetry hardware to collect real-time environmental data, applies AI algorithms for fraud detection and predictive maintenance, and provides user interfaces for both kiln owners and government administrators.

## Glossary

- **AERIS_System**: The complete AI-Verified Pollution Compliance Network platform
- **Edge_Device**: ESP32-based hardware unit deployed at kiln sites
- **Telemetry_Data**: Real-time sensor readings including smoke density, soot weight, temperature, and power consumption
- **Fraud_Detection_Engine**: AI algorithm that identifies tampering attempts
- **Carbon_Ledger**: System that tracks and awards carbon credits based on compliance
- **Kiln_Owner**: Industrial operator who owns and manages brick kilns
- **Government_Administrator**: Regulatory official monitoring compliance across multiple kilns
- **Bypass_Alert**: Notification triggered when tampering is detected
- **Carbon_Credit**: Digital reward unit earned for pollution reduction compliance
- **Predictive_Maintenance_Engine**: AI system that forecasts equipment maintenance needs
- **Mobile_App**: React Native application for kiln owners
- **Web_Dashboard**: React.js application for government administrators

## Requirements

### Requirement 1: Edge Telemetry Data Collection

**User Story:** As a kiln owner, I want real-time monitoring of my kiln's environmental parameters, so that I can track compliance and system performance.

#### Acceptance Criteria

1. WHEN the Edge_Device is powered on, THE AERIS_System SHALL initialize all sensors within 30 seconds
2. THE Edge_Device SHALL collect Smoke_Density readings from MQ135 sensor every 10 seconds
3. THE Edge_Device SHALL collect Soot_Weight measurements from Load Cell sensor every 30 seconds
4. THE Edge_Device SHALL collect Kiln_Temp readings from Thermocouple sensor every 5 seconds
5. THE Edge_Device SHALL collect Power_Current measurements from ACS712 sensor every 5 seconds
6. WHEN sensor readings are collected, THE Edge_Device SHALL transmit data to AWS IoT Core via MQTT protocol within 60 seconds
7. IF any sensor fails to provide readings, THEN THE Edge_Device SHALL log the failure and continue operating with remaining sensors

### Requirement 2: Anti-Fraud Detection System

**User Story:** As a government administrator, I want to detect tampering attempts on monitoring equipment, so that I can ensure data integrity and prevent compliance fraud.

#### Acceptance Criteria

1. WHEN Kiln_Temp exceeds 200°C AND Power_Current is below 0.5A, THE Fraud_Detection_Engine SHALL trigger a Bypass_Alert within 30 seconds
2. WHEN a Bypass_Alert is triggered, THE AERIS_System SHALL record the incident with timestamp and sensor values
3. WHEN a Bypass_Alert occurs, THE AERIS_System SHALL notify the Government_Administrator via the Web_Dashboard immediately
4. THE Fraud_Detection_Engine SHALL analyze telemetry patterns continuously for anomaly detection
5. WHEN suspicious patterns are detected, THE AERIS_System SHALL flag the kiln for manual review
6. THE AERIS_System SHALL maintain a fraud incident history for each kiln for audit purposes

### Requirement 3: Carbon Credit Reward System

**User Story:** As a kiln owner, I want to earn carbon credits for pollution reduction compliance, so that I am incentivized to maintain clean operations.

#### Acceptance Criteria

1. WHEN Soot_Weight increases by 1kg AND airflow readings are within normal parameters, THE Carbon_Ledger SHALL award 1 Carbon_Credit to the kiln owner's wallet
2. THE Carbon_Ledger SHALL validate soot collection measurements against baseline thresholds before awarding credits
3. WHEN Carbon_Credits are awarded, THE AERIS_System SHALL update the kiln owner's wallet balance immediately
4. THE Carbon_Ledger SHALL maintain an immutable transaction history of all credit awards and deductions
5. WHEN fraud is detected, THE Carbon_Ledger SHALL freeze credit awards for the affected kiln until investigation is complete
6. THE AERIS_System SHALL provide real-time credit balance updates to the Mobile_App

### Requirement 4: Predictive Maintenance System

**User Story:** As a kiln owner, I want advance warning of equipment maintenance needs, so that I can prevent system failures and maintain compliance.

#### Acceptance Criteria

1. THE Predictive_Maintenance_Engine SHALL analyze airflow resistance trends continuously
2. WHEN airflow resistance patterns indicate filter clog risk, THE Predictive_Maintenance_Engine SHALL generate a maintenance alert 48 hours in advance
3. WHEN maintenance alerts are generated, THE AERIS_System SHALL notify the kiln owner via the Mobile_App
4. THE Predictive_Maintenance_Engine SHALL learn from historical maintenance data to improve prediction accuracy
5. THE AERIS_System SHALL track maintenance completion and correlate with predicted schedules for model refinement

### Requirement 5: Mobile Application for Kiln Owners

**User Story:** As a kiln owner, I want a mobile interface to monitor my system status and credits, so that I can manage my operations effectively from anywhere.

#### Acceptance Criteria

1. WHEN a kiln owner opens the Mobile_App, THE AERIS_System SHALL display current Carbon_Credit balance within 5 seconds
2. THE Mobile_App SHALL show real-time system health status including all sensor readings
3. WHEN system alerts occur, THE Mobile_App SHALL display notifications with alert details and recommended actions
4. THE Mobile_App SHALL provide historical charts of telemetry data for the past 30 days
5. WHEN the kiln owner requests credit transaction history, THE Mobile_App SHALL display all transactions with timestamps and amounts
6. THE Mobile_App SHALL work offline and sync data when connectivity is restored

### Requirement 6: Web Dashboard for Government Administrators

**User Story:** As a government administrator, I want a comprehensive dashboard to monitor compliance across all kilns, so that I can enforce regulations effectively.

#### Acceptance Criteria

1. WHEN a Government_Administrator accesses the Web_Dashboard, THE AERIS_System SHALL display a map view of all registered kilns within 10 seconds
2. THE Web_Dashboard SHALL color-code kilns as "Compliant" (green) or "Non-Compliant" (red) based on current status
3. WHEN fraud alerts are triggered, THE Web_Dashboard SHALL highlight affected kilns with alert indicators
4. THE Web_Dashboard SHALL provide detailed drill-down views for individual kiln telemetry and compliance history
5. THE AERIS_System SHALL generate compliance reports for specified date ranges and kiln groups
6. WHEN compliance thresholds are updated, THE Web_Dashboard SHALL allow administrators to modify parameters system-wide

### Requirement 7: Data Storage and Management

**User Story:** As a system architect, I want reliable data storage and retrieval capabilities, so that the platform can handle large-scale telemetry data efficiently.

#### Acceptance Criteria

1. THE AERIS_System SHALL store all telemetry data in Amazon Timestream with automatic partitioning by timestamp
2. WHEN telemetry data is received, THE AERIS_System SHALL persist it within 30 seconds of receipt
3. THE AERIS_System SHALL retain raw telemetry data for 2 years for compliance auditing
4. WHEN data queries are requested, THE AERIS_System SHALL return results within 5 seconds for standard time ranges
5. THE AERIS_System SHALL implement data backup and disaster recovery procedures with 99.9% availability
6. THE AERIS_System SHALL compress historical data older than 90 days to optimize storage costs

### Requirement 8: AI Model Training and Deployment

**User Story:** As a data scientist, I want to train and deploy AI models for fraud detection and predictive maintenance, so that the system can continuously improve its accuracy.

#### Acceptance Criteria

1. THE AERIS_System SHALL use Amazon SageMaker for training fraud detection models on historical telemetry patterns
2. WHEN new training data becomes available, THE AERIS_System SHALL retrain models monthly to maintain accuracy
3. THE AERIS_System SHALL deploy updated models to production with zero downtime using blue-green deployment
4. WHEN model performance degrades below 85% accuracy, THE AERIS_System SHALL trigger automatic retraining
5. THE AERIS_System SHALL maintain model versioning and rollback capabilities for production stability
6. THE AERIS_System SHALL log all model predictions and outcomes for continuous learning improvement