---
title: "Getting Started"
description: "Start building with Nexus Framework"
weight: 2
---

# Getting Started with Nexus

This guide will help you set up and start using the Nexus Framework in your projects.

## Prerequisites

- Go 1.23.4 or higher
- Git
- Docker (optional)

## Installation

### Using Go

```bash
# Install the Nexus Framework
go get github.com/gavinvolpe/nexus

# Initialize a new project
mkdir myproject && cd myproject
go mod init myproject
```

### Using Docker

```bash
# Pull the Nexus image
docker pull gavinvolpe/nexus

# Run a Nexus container
docker run -p 8080:8080 gavinvolpe/nexus
```

## Basic Usage

### 1. Initialize Nexus

```go
package main

import (
    "context"
    "github.com/gavinvolpe/nexus"
)

func main() {
    // Create a new Nexus instance
    nx := nexus.New(nexus.Config{
        Provider:    nexus.OpenAI,
        ModelID:     "gpt-4",
        Temperature: 0.7,
    })

    // Initialize agents
    mentor := nx.NewMentorAgent()
    mentee := nx.NewMenteeAgent()
}
```

### 2. Create a Task

```go
task := nexus.Task{
    Description: "Create a REST API endpoint",
    Requirements: []string{
        "Handle POST requests",
        "Accept JSON payload",
        "Return 200 status on success",
    },
    Context: map[string]interface{}{
        "framework": "gin",
        "database": "postgresql",
    },
}
```

### 3. Execute the Task

```go
// Propose a solution
solution := mentee.ProposeSolution(context.Background(), task)

// Get mentor feedback
feedback := mentor.ReviewSolution(context.Background(), solution)

// Implement final solution
if feedback.IsApproved() {
    result := mentee.Implement(context.Background(), solution)
    fmt.Println("Implementation successful:", result)
}
```

## Configuration

### Environment Variables

```bash
NEXUS_API_KEY=your-api-key
NEXUS_MODEL_ID=gpt-4
NEXUS_ENDPOINT=https://api.nexus.dev
```

### Configuration File

```yaml
# nexus.yaml
provider: openai
model_id: gpt-4
temperature: 0.7
max_tokens: 2000
top_p: 1.0
```

## Next Steps

1. Explore the [Core Concepts](/docs/concepts)
2. Learn about [Components](/docs/components)
3. Read the [API Reference](/docs/api)
4. Join our [Community](/community)

## Common Issues

### Rate Limiting

Nexus implements rate limiting to prevent API abuse:

```go
config := nexus.Config{
    RateLimit: &nexus.RateLimit{
        RequestsPerMinute: 60,
        BurstSize: 10,
    },
}
```

### Error Handling

```go
solution, err := mentee.ProposeSolution(ctx, task)
if err != nil {
    switch e := err.(type) {
    case *nexus.ValidationError:
        fmt.Printf("Validation failed: %v\n", e)
    case *nexus.RateLimitError:
        fmt.Printf("Rate limit exceeded: %v\n", e)
    default:
        fmt.Printf("Unknown error: %v\n", err)
    }
}
```

## Support

Need help? Check out our:
- [Discord Server](https://discord.gg/nexus)
- [GitHub Issues](https://github.com/gavinvolpe/nexus/issues)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/nexus-framework)
