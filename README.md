# JMeter HTML Report Generator

A powerful, modern web application for generating comprehensive HTML reports from JMeter test results with both web interface and command-line support.

## 🚀 Features

- **📊 Comprehensive Reports**: Generate detailed HTML reports with interactive charts, tables, and analysis
- **🔄 Performance Comparison**: Compare two test runs side-by-side with detailed metrics analysis
- **🌐 Modern Web Interface**: User-friendly drag-and-drop interface built with React and TypeScript
- **⚡ CLI Support**: Direct command-line processing for automation and CI/CD integration
- **📈 Interactive Charts**: Response times, throughput, errors over time with Chart.js
- **🎯 SLA Gates**: Configurable performance thresholds with pass/fail indicators
- **🚨 Advanced Error Analysis**: Detailed error reporting with status code grouping
- **📱 Responsive Design**: Works seamlessly on desktop and mobile devices
- **🔧 Highly Configurable**: Customizable SLA thresholds, sampling rates, and report options

## 📋 Table of Contents

- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage](#usage)
  - [Web Interface](#web-interface)
  - [Command Line Interface](#command-line-interface)
- [Supported File Formats](#supported-file-formats)
- [Report Contents](#report-contents)
- [Configuration](#configuration)
- [CI/CD Integration](#cicd-integration)
- [Development](#development)
- [Troubleshooting](#troubleshooting)

## 🚀 Quick Start

### Web Interface
```bash
# Clone and install
git clone <repository-url>
cd jmeter-report-generator
npm install

# Start development server
npm run dev
# Open http://localhost:5173 in your browser
```

### Command Line Interface
```bash
# Generate report from JTL file
npm run generate-report -- -i path/to/your/test-results.jtl

# Generate comparison report
npm run generate-report -- -c current-test.jtl -p baseline-test.jtl

# Custom output location
npm run generate-report -- -i results.jtl -o custom-report.html
```

## 📦 Installation

### Prerequisites
- Node.js (version 16 or higher)
- npm or yarn package manager

### Setup
```bash
# Clone the repository
git clone <repository-url>
cd jmeter-report-generator

# Install dependencies
npm install

# Start development server
npm run dev
```

## 📖 Usage

### Web Interface

1. **Single Report Generation**:
   - Navigate to the "Single Report" tab
   - Drag and drop your JTL/CSV file or click "Choose File"
   - Configure advanced settings (optional)
   - Wait for processing to complete
   - Download the generated HTML report

2. **Performance Comparison**:
   - Navigate to the "Comparison" tab
   - Upload current test file and previous/baseline test file
   - Configure comparison thresholds (optional)
   - Review the comparison analysis
   - Download the comparison report

### Command Line Interface

#### Single Report Generation
```bash
# Basic usage
npm run generate-report -- -i ./load-test.jtl

# Custom output file
npm run generate-report -- -i ./performance-test.csv -o ./reports/performance-report.html

# With configuration file
npm run generate-report -- -i ./test-results.jtl -f ./config.json

# Full path example
npm run generate-report -- --input /home/user/jmeter-results.jtl --output /home/user/reports/report.html
```

#### Comparison Report Generation
```bash
# Basic comparison
npm run generate-report -- -c ./current-test.jtl -p ./baseline-test.jtl

# Custom output for comparison
npm run generate-report -- --current-input ./new-results.csv --previous-input ./old-results.csv -o ./comparison-report.html

# With configuration file
npm run generate-report -- -c ./current-test.jtl -p ./baseline-test.jtl -f ./config.json
```

#### CLI Options
```
Single Report:
  -i, --input <file>           Path to JMeter JTL/CSV file (required)
  -o, --output <file>          Output HTML file path (optional)
  -f, --config-file <file>     Path to JSON configuration file (optional)

Comparison Report:
  -c, --current-input <file>   Path to current/new JMeter JTL/CSV file
  -p, --previous-input <file>  Path to previous/baseline JMeter JTL/CSV file
  -o, --output <file>          Output HTML file path (optional)
  -f, --config-file <file>     Path to JSON configuration file (optional)

General:
  -h, --help                   Show help message
```

## 📄 Supported File Formats

### JTL Files
- **XML Format**: Standard JMeter XML output with `<testResults>` root element
- **CSV Format**: JMeter CSV output with headers

### CSV Files
Must contain the following required columns:
- `timeStamp` - Request timestamp (milliseconds)
- `elapsed` - Response time in milliseconds
- `label` - Transaction/request name
- `success` - Boolean indicating request success (`true`/`false`)

Optional columns (enhance reporting):
- `responseCode` - HTTP response code
- `responseMessage` - Error message
- `threadName` - Thread/user identifier
- `bytes` - Response size in bytes
- `sentBytes` - Request size in bytes
- `grpThreads` - Group thread count
- `allThreads` - Total active threads

## 📊 Report Contents

### Dashboard Tab
- **Test Configuration Summary**: Duration, users, environment details
- **SLA Gates Status**: Pass/fail indicators for performance thresholds
- **Key Performance Metrics**: Throughput, response times, error rates
- **Aggregate Report Table**: Detailed transaction-level statistics

### Graphs Tab
- **Time-Series Charts**:
  - Response Times Over Time (by transaction)
  - Throughput Analysis
  - Error Rate Trends
  - Hits Per Second
- **Statistical Analysis**:
  - Response Time Percentiles (50th, 75th, 90th, 95th, 99th)
- **Correlation Analysis**:
  - Throughput vs Response Time
  - Users vs Response Time
  - Error Analysis Scatter Plots

### Error Report Tab
- **Error Summary by HTTP Status Code**
- **Detailed Error Investigation**:
  - Timestamp analysis
  - Thread information
  - Error messages and codes
  - Transaction-level error breakdown

### Comparison Tab (when available)
- **Side-by-Side Test Comparison**
- **Performance Change Analysis**
- **Improvement/Regression Identification**
- **Detailed Metrics Comparison**
- **Insights and Recommendations**

## ⚙️ Configuration

### Configuration File Format (JSON)
```json
{
  "slaThresholds": {
    "avgResponseTime": 3000,
    "throughput": 0.03055,
    "errorRate": 10
  },
  "reportOptions": {
    "includeCharts": true,
    "maxErrorSamples": 200,
    "samplingRate": 2
  },
  "testConfig": {
    "testEnvironment": "PPE02"
  }
}
```

### Configuration Options

#### SLA Thresholds
- `avgResponseTime`: Maximum acceptable average response time (ms)
- `throughput`: Minimum acceptable throughput (requests per second)
- `errorRate`: Maximum acceptable error rate (percentage)

#### Report Options
- `includeCharts`: Include interactive charts in HTML report
- `maxErrorSamples`: Maximum number of error samples to include
- `samplingRate`: Chart data sampling rate (higher = faster processing)

#### Test Configuration
- `testEnvironment`: Environment identifier for the test

## 🔄 CI/CD Integration

### Jenkins Integration
```groovy
pipeline {
    stages {
        stage('Performance Test') {
            steps {
                // Run JMeter tests
                sh 'jmeter -n -t test-plan.jmx -l results.jtl'
                
                // Generate HTML report
                sh 'npm run generate-report -- -i results.jtl -o jmeter-report.html'
                
                // Publish HTML report
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'jmeter-report.html',
                    reportName: 'JMeter Performance Report'
                ])
            }
        }
    }
}
```

### GitHub Actions
```yaml
name: Performance Testing
on: [push, pull_request]

jobs:
  performance-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm install
        
      - name: Run JMeter tests
        run: jmeter -n -t test-plan.jmx -l test-results.jtl
        
      - name: Generate performance report
        run: npm run generate-report -- -i test-results.jtl -o performance-report.html
        
      - name: Upload performance report
        uses: actions/upload-artifact@v3
        with:
          name: performance-report
          path: performance-report.html
```

### GitLab CI
```yaml
performance_test:
  stage: test
  script:
    - npm install
    - jmeter -n -t test-plan.jmx -l results.jtl
    - npm run generate-report -- -i results.jtl -o jmeter-report.html
  artifacts:
    reports:
      performance: jmeter-report.html
    paths:
      - jmeter-report.html
    expire_in: 1 week
```

## 🛠️ Development

### Project Structure
```
src/
├── components/          # React components
│   ├── charts/         # Chart components
│   ├── FileUploader.tsx
│   ├── ReportDashboard.tsx
│   └── ...
├── types/              # TypeScript type definitions
├── utils/              # Utility functions
│   ├── jmeterParser.ts
│   ├── reportGenerator.ts
│   └── comparisonAnalyzer.ts
├── cli.ts              # Command-line interface
└── App.tsx             # Main application
```

### Available Scripts
```bash
# Development
npm run dev              # Start development server
npm run build           # Build for production
npm run preview         # Preview production build

# Code Quality
npm run lint            # Run ESLint
npm run typecheck       # Run TypeScript type checking

# Report Generation
npm run generate-report # CLI report generation
```

### Technology Stack
- **Frontend**: React 18, TypeScript, Tailwind CSS
- **Charts**: Chart.js with date-fns adapter
- **Data Processing**: PapaParse, Lodash, Simple Statistics
- **Build Tool**: Vite
- **CLI**: Node.js with TypeScript execution

## 🔧 Troubleshooting

### Common Issues

#### File Processing Errors
**"No valid samples found"**
- Verify file contains JMeter test results
- Check required columns are present (`timeStamp`, `elapsed`, `label`, `success`)
- Ensure file is not empty or corrupted

**"XML parsing failed"**
- Verify JTL file is valid XML format
- Check file wasn't truncated during test execution
- Try opening file in text editor to verify format

**"File not found"**
- Check file path is correct and accessible
- Use absolute paths if relative paths don't work
- Verify file permissions

#### Performance Issues
**Large file processing is slow**
- Files >100MB may take several minutes to process
- Increase sampling rate in advanced settings
- Consider splitting very large files

**Browser memory issues**
- Use CLI for very large files (>200MB)
- Increase browser memory limits if possible
- Process files in smaller chunks

#### Report Generation Issues
**Charts not displaying**
- Ensure JavaScript is enabled in browser
- Check browser console for errors
- Verify Chart.js libraries loaded correctly

**Missing data in reports**
- Check if all required columns are present
- Verify data format matches expected structure
- Review sampling settings if data appears incomplete

### Performance Tips
- Use sampling options for datasets >1M samples
- Enable chart sampling for faster rendering
- Configure appropriate error sample limits
- Use CLI for automated/batch processing

### Getting Help
1. Check this README for common solutions
2. Review the troubleshooting section
3. Check browser console for error messages
4. Verify file format and required columns
5. Test with the provided sample file (`test-sample.jtl`)

## 📈 Performance Recommendations

### File Size Guidelines
- **Small files** (<10MB): All features enabled, minimal sampling
- **Medium files** (10-50MB): Moderate sampling, full chart generation
- **Large files** (50-200MB): Increased sampling, selective chart generation
- **Very large files** (>200MB): CLI recommended, high sampling rates

### Best Practices
- Use consistent test environments for comparisons
- Ensure transaction names match exactly for comparisons
- Include sufficient samples per transaction (minimum 10-50)
- Configure appropriate SLA thresholds for your application
- Regular cleanup of old test results and reports

## 📄 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request