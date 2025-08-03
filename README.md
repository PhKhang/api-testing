# API Testing with Newman

This project contains automated API tests for invoice management using Postman collections and Newman.

## Collections

- **Get invoices.postman_collection.json**: Tests for retrieving invoices
- **Invoices adding.postman_collection.json**: Tests for adding new invoices

## Test Data

- **login_details.json**: Contains authentication scenarios (authorized/unauthorized)
- **invoice_details.json**: Contains test data for invoice creation

## Local Testing

### Prerequisites

- Node.js (v18 or higher)
- npm

### Setup

1. Install dependencies:
   ```bash
   npm install
   ```

2. Install Newman globally:
   ```bash
   npm install -g newman newman-reporter-htmlextra
   ```

### Running Tests

Run the Get Invoices collection:
```bash
newman run "Get invoices.postman_collection.json" --environment login_details.json
```

Run the Invoices Adding collection:
```bash
newman run "Invoices adding.postman_collection.json" --environment login_details.json --globals invoice_details.json
```

Generate HTML reports:
```bash
newman run "Get invoices.postman_collection.json" --environment login_details.json --reporters cli,htmlextra --reporter-htmlextra-export get-invoices-report.html

newman run "Invoices adding.postman_collection.json" --environment login_details.json --globals invoice_details.json --reporters cli,htmlextra --reporter-htmlextra-export invoices-adding-report.html
```

## CI/CD with GitHub Actions

The project includes a GitHub Actions workflow (`.github/workflows/api-tests.yml`) that:

1. **Triggers on**:
   - Push to `main` branch
   - Pull requests to `main` branch

2. **Test Execution**:
   - Runs both Postman collections using Newman
   - Uses `login_details.json` as environment variables
   - Uses `invoice_details.json` as global variables for invoice data

3. **Reporting**:
   - Generates HTML reports for each collection
   - Uploads reports as GitHub Actions artifacts
   - Comments on pull requests with test results summary

4. **Failure Handling**:
   - Uses `--bail` flag to stop on first failure
   - Workflow fails if any tests fail
   - Reports are still uploaded even on failure

## Test Data Structure

### login_details.json
```json
[
  {
    "scenario": "unauthorized",
    "email": "",
    "password": "",
    "token": "invalid_token"
  },
  {
    "scenario": "authorized",
    "email": "customer2@practicesoftwaretesting.com",
    "password": "welcome01",
    "token": ""
  }
]
```

### invoice_details.json
```json
[
  {
    "total": 0,
    "invoice_items": [
      {
        "product_id": 2,
        "quantity": 10,
        "unit_price": 39.01
      }
    ]
  }
]
```

## Workflow Features

- ✅ Automated testing on code changes
- 📊 HTML test reports with detailed results
- 🚀 Fast feedback on pull requests
- 📁 Artifact storage for test reports (30 days retention)
- 💬 Automatic PR comments with test summaries
- ⚡ Fail-fast approach to catch issues early

## Customization

To modify the workflow:

1. **Change trigger conditions**: Edit the `on:` section in the workflow file
2. **Add environment variables**: Use GitHub Secrets for sensitive data
3. **Modify test execution**: Update the Newman command parameters
4. **Change reporting**: Modify the reporter options or add new reporters
