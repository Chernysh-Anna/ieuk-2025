# Web Traffic Log Analysis Tool

A Python-based tool for analyzing web server logs to detect bot traffic and traffic patterns. This tool helps identify suspicious IP addresses, analyze server performance metrics, and generate reports.

## Features

- **Bot Detection**: Identifies suspicious bot traffic based on multiple indicators
- **Performance Analysis**: Analyzes response times, error rates, and server load
- **Traffic Pattern Analysis**: Identifies peak hours and popular pages
- **Suspicious IP Export**: Exports detected bot IPs to CSV 
- **Reporting**: Generates detailed analysis reports
- **Docker Support**: Containerized deployment with Docker

## Project Structure

```
log-analysis-project/
├── IEUK-TASK/
├── logs/
│   └── sample-log.log
├── output/
│   └── suspicious_ips.csv
├── scripts/
│   └── setup.sh
├── docker-compose.yml
├── Dockerfile
├── log_analyzer.py
├── README.md
├── requirements.txt
└── run_tests.py
```

## Installation

### Prerequisites

- Python 3.11+
- Docker

### Setup

1. **Clone the repository:**
```bash
git clone <repository-url>
cd log-analysis-project
```

2. **Run setup script:**
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

3. **Install dependencies manually:**
```bash
pip install -r requirements.txt
```

## Usage

### Command Line

#### Basic Analysis
```bash
python log_analyzer.py logs/sample-log.log
```

#### Generate Report Only
```bash
python log_analyzer.py logs/sample-log.log --report-only
```

#### Export Suspicious IPs to Custom File
```bash
python log_analyzer.py logs/sample-log.log --export-csv output/blocked_ips.csv
```

### Docker Usage

#### Build Docker Image Manually
```bash
docker build -t log-analyzer .
docker run -v $(pwd)/logs:/app/logs -v $(pwd)/output:/app/output log-analyzer python log_analyzer.py /app/logs/sample-log.log
```

## Log Format

The tool expects web server logs in the following format:
```
IP - COUNTRY - [TIMESTAMP] "METHOD URL HTTP/VERSION" STATUS SIZE "REFERER" "USER_AGENT" RESPONSE_TIME
```


## Bot Detection Criteria

- **High Request Volume**: >100 requests (score: +3), >50 requests (score: +2)
- **Low URL Diversity**: <30% unique URLs (score: +2)
- **Bot User Agents**: Matches known bot patterns (score: +4)
- **Regular Intervals**: Very consistent request timing (score: +2)
- **High Error Rate**: >50% HTTP errors (score: +1)
- **Single User Agent**: Only one user agent with >20 requests (score: +1)
- **Suspicious URLs**: Admin panels, config files, etc. (score: +2)

**Bot Classification Threshold**: Score ≥ 4

## Output

### Console Report
```
=== WEB TRAFFIC ANALYSIS REPORT ===

PERFORMANCE METRICS:
Total Requests: ---
Average Response Time: ---ms
95th Percentile Response Time: ---ms
Error Rate: --%

TOP REQUESTED PAGES:
  /: --- requests
 

SUSPECTED BOT TRAFFIC:
Found - suspicious IP addresses

IP: 192.168.1.1 (Score: 8)
  Requests: ---
  Countries: --
  Indicators:
    - High request volume: ---
    - Bot user agent: ---
    - Suspicious URL: ---

PEAK TRAFFIC HOURS:
  14:00: --- requests
  13:00: --- requests
  15:00: --- requests
```

### CSV Export
Suspicious IPs are exported to `suspicious_ips.csv` with columns:
- IP Address
- Bot Score
- Request Count
- Countries
- Primary Indicator

## Testing

Run the test suite:
```bash
python run_tests.py
```

## Configuration

### Bot User Agent Patterns
Edit `self.bot_user_agents` in `LogAnalyzer.__init__()` to customize bot detection patterns.

### Suspicious URL Patterns
Edit `self.suspicious_patterns` in `LogAnalyzer.__init__()` to add custom suspicious URL patterns.

### Detection Thresholds
Modify scoring thresholds in `detect_bot_behavior()` method for different sensitivity levels.

## Dependencies

- `re`: Regular expression matching
- `csv`: CSV file handling
- `collections`: Data structures (defaultdict, Counter)
- `datetime`: Date/time parsing
- `argparse`: Command-line argument parsing
- `statistics`: Statistical calculations
- `typing`: Type hints

## Troubleshooting

### Common Issues

1. **"Log file not found"**
   - Ensure log file path is correct
   - Check file permissions

2. **"Could not parse line"**
   - Verify log format matches expected pattern
   - Check for encoding issues




