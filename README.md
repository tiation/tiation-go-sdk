# Tiation Go SDK

<div align="center">
  <img src="https://via.placeholder.com/800x200/1a1a2e/16213e?text=Tiation+Go+SDK" alt="Tiation Go SDK Banner" />
  
  <h3>Official Go SDK for Tiation APIs</h3>
  
  [![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/tiation/tiation-go-sdk)
  [![Go](https://img.shields.io/badge/go-1.19+-brightgreen.svg)](https://golang.org)
  [![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
  [![Enterprise](https://img.shields.io/badge/enterprise-ready-purple.svg)](https://github.com/tiation)
</div>

## 🌟 Overview

The Tiation Go SDK provides a high-performance, enterprise-grade interface for interacting with all Tiation services. Built with Go's concurrency model and designed for scalability, it offers seamless integration with your Go applications.

## ✨ Features

- **🚀 High Performance**: Optimized for speed with Go's concurrency
- **🔒 Enterprise Security**: Built-in authentication and rate limiting
- **📊 Real-time Analytics**: Live metrics and business intelligence
- **🔄 Concurrent Operations**: Full support for goroutines and channels
- **🛡️ Type Safety**: Strongly typed interfaces and error handling
- **📚 Rich Documentation**: Comprehensive docs with examples
- **⚡ Low Latency**: Optimized for high-throughput applications

## 📦 Installation

```bash
go get github.com/tiation/tiation-go-sdk
```

## 🚀 Quick Start

```go
package main

import (
    "context"
    "fmt"
    "log"
    
    "github.com/tiation/tiation-go-sdk/tiation"
)

func main() {
    // Initialize client
    client := tiation.NewClient("your_api_key")
    
    // Get business analytics
    ctx := context.Background()
    analytics, err := client.Analytics.GetMetrics(ctx)
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Printf("Total revenue: $%.2f\n", analytics.Revenue)
    
    // Create automation workflow
    workflow := &tiation.Workflow{
        Name:    "Customer Onboarding",
        Trigger: "user_signup",
        Actions: []string{"send_welcome_email", "create_profile"},
    }
    
    result, err := client.Automation.CreateWorkflow(ctx, workflow)
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Printf("Workflow created: %s\n", result.ID)
}
```

## 📚 Documentation

### Authentication

```go
import "github.com/tiation/tiation-go-sdk/tiation"

// API Key authentication
client := tiation.NewClient("your_api_key")

// OAuth2 authentication
config := &tiation.Config{
    ClientID:     "your_client_id",
    ClientSecret: "your_client_secret",
    OAuthToken:   "your_oauth_token",
}
client := tiation.NewClientWithConfig(config)
```

### Core Services

#### Analytics & Metrics
```go
// Get business metrics
metrics, err := client.Analytics.GetMetrics(ctx, &tiation.MetricsRequest{
    StartDate: "2024-01-01",
    EndDate:   "2024-12-31",
})

// Real-time dashboard data
dashboard, err := client.Analytics.GetDashboard(ctx)
```

#### Automation Workflows
```go
// List workflows
workflows, err := client.Automation.ListWorkflows(ctx)

// Execute workflow
result, err := client.Automation.ExecuteWorkflow(ctx, &tiation.ExecuteRequest{
    WorkflowID: "workflow_123",
    Parameters: map[string]interface{}{
        "user_id": "user_456",
    },
})
```

#### Content Management
```go
// Create content
content := &tiation.Content{
    Title:    "New Product Launch",
    Body:     "Exciting new features available now!",
    Category: "announcements",
}
result, err := client.CMS.CreateContent(ctx, content)

// Update content
err = client.CMS.UpdateContent(ctx, result.ID, &tiation.ContentUpdate{
    Status: "published",
})
```

### Advanced Features

#### Concurrent Operations
```go
import "sync"

func processConcurrently(client *tiation.Client, ctx context.Context) {
    var wg sync.WaitGroup
    
    // Concurrent API calls
    wg.Add(3)
    
    go func() {
        defer wg.Done()
        metrics, _ := client.Analytics.GetMetrics(ctx)
        // Process metrics
    }()
    
    go func() {
        defer wg.Done()
        workflows, _ := client.Automation.ListWorkflows(ctx)
        // Process workflows
    }()
    
    go func() {
        defer wg.Done()
        content, _ := client.CMS.ListContent(ctx)
        // Process content
    }()
    
    wg.Wait()
}
```

#### Error Handling
```go
import "github.com/tiation/tiation-go-sdk/tiation"

metrics, err := client.Analytics.GetMetrics(ctx)
if err != nil {
    switch e := err.(type) {
    case *tiation.RateLimitError:
        fmt.Printf("Rate limit exceeded, retry after: %v\n", e.RetryAfter)
    case *tiation.APIError:
        fmt.Printf("API error: %s (code: %d)\n", e.Message, e.Code)
    default:
        fmt.Printf("Unexpected error: %v\n", err)
    }
}
```

## 🔧 Configuration

### Environment Variables
```bash
export TIATION_API_KEY="your_api_key"
export TIATION_BASE_URL="https://api.tiation.com"
export TIATION_TIMEOUT=30
```

### Configuration Struct
```go
config := &tiation.Config{
    APIKey:     "your_api_key",
    BaseURL:    "https://api.tiation.com",
    Timeout:    30 * time.Second,
    MaxRetries: 3,
    RetryDelay: 1 * time.Second,
}

client := tiation.NewClientWithConfig(config)
```

## 🏢 Enterprise Features

### Batch Operations
```go
// Batch create content
contents := []*tiation.Content{
    {Title: "Article 1", Body: "Content 1"},
    {Title: "Article 2", Body: "Content 2"},
}
results, err := client.CMS.BatchCreate(ctx, contents)

// Batch analytics
requests := []*tiation.MetricsRequest{
    {Metric: "revenue", Period: "monthly"},
    {Metric: "users", Period: "daily"},
}
metrics, err := client.Analytics.BatchMetrics(ctx, requests)
```

### Webhook Integration
```go
import "github.com/tiation/tiation-go-sdk/webhooks"

processor := webhooks.NewProcessor("your_webhook_secret")

processor.HandleEvent("user.created", func(event *webhooks.Event) error {
    fmt.Printf("New user: %s\n", event.Data["email"])
    
    // Trigger welcome workflow
    _, err := client.Automation.ExecuteWorkflow(ctx, &tiation.ExecuteRequest{
        WorkflowID: "welcome_workflow",
        Parameters: map[string]interface{}{
            "user_id": event.Data["id"],
        },
    })
    
    return err
})
```

## 🧪 Testing

```bash
# Run tests
go test ./...

# Run with coverage
go test -cover ./...

# Run specific test
go test -run TestAnalyticsGetMetrics ./analytics
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

```bash
# Clone repository
git clone https://github.com/tiation/tiation-go-sdk.git
cd tiation-go-sdk

# Install dependencies
go mod tidy

# Run tests
go test ./...
```

## 📞 Support

- **Documentation**: [Go SDK Docs](https://docs.tiation.com/go-sdk)
- **API Reference**: [API Documentation](https://api.tiation.com/docs)
- **Issues**: [GitHub Issues](https://github.com/tiation/tiation-go-sdk/issues)
- **Enterprise Support**: [tiatheone@protonmail.com](mailto:tiatheone@protonmail.com)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">
  <p>Built with ❤️ by <a href="https://github.com/tiation">Tiation</a></p>
  <p>High-performance Go SDK for enterprise applications</p>
</div>
