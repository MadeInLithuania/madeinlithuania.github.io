# Logging System

This document provides comprehensive information about Riceify's logging system, including configuration, log levels, and debugging techniques.

## 📋 Overview

Riceify implements a robust, thread-safe logging system designed for:
- **Performance monitoring** - Track operation speeds and bottlenecks
- **Debugging** - Comprehensive error reporting and stack traces
- **Audit trails** - Complete operation history for troubleshooting
- **Real-time monitoring** - Live log streaming and analysis

## 🏗️ Architecture

### Logging Components

```cpp
// Core logging structure
class Logger {
private:
    std::mutex log_mutex;
    std::ofstream log_file;
    LogLevel current_level;
    std::string log_format;
    bool thread_safe;
    
public:
    void log(LogLevel level, const std::string& message);
    void set_level(LogLevel level);
    void set_format(const std::string& format);
};
```

### Log Levels

Riceify supports multiple log levels with increasing verbosity:

| Level | Numeric | Description | Use Case |
|-------|---------|-------------|----------|
| **FATAL** | 0 | Critical errors | System failures, crashes |
| **ERROR** | 1 | Error conditions | Operation failures |
| **WARN** | 2 | Warning messages | Potential issues |
| **INFO** | 3 | General information | Normal operations |
| **DEBUG** | 4 | Debug information | Development debugging |
| **TRACE** | 5 | Detailed tracing | Fine-grained debugging |

### Thread Safety

The logging system is fully thread-safe:

```cpp
// Thread-safe logging example
std::mutex log_mutex;

void log_message(const std::string& message) {
    std::lock_guard<std::mutex> lock(log_mutex);
    // Log the message safely
    write_to_log(message);
}
```

## ⚙️ Configuration

### Basic Configuration

Configure logging in `~/.config/riceify/config.toml`:

```toml
[logging]
# Log level (fatal, error, warn, info, debug, trace)
level = "info"

# Enable file logging
file_logging = true

# Enable console logging
console_logging = true

# Log file path
log_file = "~/.local/share/riceify/riceify.log"

# Maximum log file size (MB)
max_file_size = 100

# Number of backup files to keep
backup_count = 5

# Log format
format = "[{timestamp}] [{level}] [{thread}] {message}"

# Enable performance logging
performance_logging = true

# Enable operation tracing
operation_tracing = true
```

### Advanced Configuration

```toml
[logging.advanced]
# Enable structured logging (JSON format)
structured_logging = false

# Enable log compression
compress_logs = true

# Log rotation schedule
rotation_schedule = "daily"  # daily, weekly, monthly

# Enable remote logging
remote_logging = false
remote_endpoint = "https://logs.riceify.com"

# Enable log encryption
encrypt_logs = false
encryption_key = ""

# Performance thresholds
slow_operation_threshold = 1000  # milliseconds
memory_warning_threshold = 512   # MB
```

## 📝 Log Format

### Default Format

```
[2024-01-15 14:30:25.123] [INFO] [main] Starting rice save operation
[2024-01-15 14:30:25.124] [DEBUG] [worker-1] Processing file: ~/.config/i3/config
[2024-01-15 14:30:25.125] [DEBUG] [worker-2] Processing file: ~/.config/polybar/config.ini
[2024-01-15 14:30:25.126] [INFO] [main] Rice save completed in 3ms
```

### Custom Formats

Configure custom log formats using placeholders:

```toml
# Simple format
format = "{level}: {message}"

# Detailed format with timestamps
format = "[{timestamp}] [{level}] [{thread}] [{function}] {message}"

# JSON format
format = '{"timestamp":"{timestamp}","level":"{level}","thread":"{thread}","message":"{message}"}'
```

**Available Placeholders:**
- `{timestamp}` - Current timestamp
- `{level}` - Log level
- `{thread}` - Thread ID/name
- `{function}` - Function name
- `{file}` - Source file
- `{line}` - Line number
- `{message}` - Log message
- `{pid}` - Process ID
- `{memory}` - Current memory usage

## 🔍 Log Analysis

### Built-in Analysis Tools

Riceify includes tools for log analysis:

```bash
# View recent logs
riceify log --recent --lines 50

# Filter logs by level
riceify log --level error,warn

# Search logs
riceify log --search "backup failed"

# Show performance summary
riceify log --performance

# Generate log report
riceify log --report --output report.html
```

### Performance Analysis

Analyze performance from logs:

```bash
# Show slowest operations
riceify log --slowest --limit 10

# Show memory usage over time
riceify log --memory --graph

# Show cache hit rates
riceify log --cache-stats

# Show error frequency
riceify log --error-stats
```

### Log Parsing Examples

```bash
# Count log entries by level
grep -E "\[(ERROR|WARN|INFO|DEBUG)\]" riceify.log | cut -d' ' -f3 | sort | uniq -c

# Find slow operations (>1 second)
grep "completed in" riceify.log | awk '$NF > 1000 {print $0}'

# Extract error messages
grep "\[ERROR\]" riceify.log | cut -d' ' -f4-

# Show recent errors with context
grep -A5 -B5 "\[ERROR\]" riceify.log | tail -20
```

## 🐛 Debugging with Logs

### Common Debug Scenarios

1. **Rice switching fails**
   ```bash
   # Check for permission errors
   riceify log --search "permission denied"
   
   # Check for missing files
   riceify log --search "file not found"
   
   # Check backup operations
   riceify log --search "backup"
   ```

2. **Performance issues**
   ```bash
   # Find slow operations
   riceify log --slowest
   
   # Check memory usage
   riceify log --memory
   
   # Check cache performance
   riceify log --cache-stats
   ```

3. **Configuration problems**
   ```bash
   # Check config loading
   riceify log --search "config"
   
   # Check validation errors
   riceify log --search "validation"
   ```

### Debug Commands

```bash
# Enable debug logging
riceify --debug

# Enable trace logging
riceify --trace

# Log to specific file
riceify --log-file debug.log

# Show log in real-time
riceify --log-follow

# Clear logs
riceify --clear-logs
```

## 📊 Log Metrics

### Key Metrics

Monitor these metrics for system health:

- **Error rate** - Percentage of error logs
- **Response time** - Operation completion times
- **Memory usage** - Peak and average memory
- **Cache efficiency** - Hit/miss ratios
- **File operations** - Read/write counts

### Alerting

Set up alerts for critical conditions:

```toml
[logging.alerts]
# Alert on high error rate
error_rate_threshold = 5.0  # percentage

# Alert on slow operations
slow_operation_threshold = 5000  # milliseconds

# Alert on memory usage
memory_threshold = 1024  # MB

# Alert on disk space
disk_space_threshold = 90  # percentage
```

## 🔧 Integration

### External Log Aggregators

Riceify can integrate with external logging systems:

```toml
[logging.external]
# Syslog integration
syslog_enabled = true
syslog_facility = "local0"

# Journald integration
journald_enabled = true

# ELK stack integration
elasticsearch_enabled = false
elasticsearch_url = "http://localhost:9200"

# Prometheus metrics
prometheus_enabled = false
prometheus_port = 9090
```

### Log Shipping

Configure log shipping to external systems:

```bash
# Send logs to remote server
riceify log --ship --endpoint https://logs.example.com

# Compress logs before shipping
riceify log --ship --compress

# Filter logs before shipping
riceify log --ship --filter "level:error,warn"
```

## 🛠️ Development

### Adding Custom Loggers

Extend the logging system with custom loggers:

```cpp
class CustomLogger : public Logger {
public:
    void log(LogLevel level, const std::string& message) override {
        // Custom logging logic
        std::cout << "CUSTOM: " << message << std::endl;
        
        // Call parent logger
        Logger::log(level, message);
    }
};
```

### Log Macros

Use convenient logging macros:

```cpp
#define LOG_TRACE(msg) logger.log(LogLevel::TRACE, msg)
#define LOG_DEBUG(msg) logger.log(LogLevel::DEBUG, msg)
#define LOG_INFO(msg)  logger.log(LogLevel::INFO, msg)
#define LOG_WARN(msg)  logger.log(LogLevel::WARN, msg)
#define LOG_ERROR(msg) logger.log(LogLevel::ERROR, msg)
#define LOG_FATAL(msg) logger.log(LogLevel::FATAL, msg)
```

## 📚 Best Practices

### Logging Guidelines

1. **Use appropriate log levels**
   - ERROR: For actual errors that affect functionality
   - WARN: For potential issues that don't break operation
   - INFO: For important operational events
   - DEBUG: For detailed debugging information

2. **Include context**
   - Always include relevant file paths, operation names, and parameters
   - Use structured logging for complex data
   - Include timestamps and thread information

3. **Performance considerations**
   - Avoid expensive operations in log statements
   - Use lazy evaluation for debug logs
   - Consider log level filtering in production

4. **Security**
   - Never log sensitive information (passwords, keys)
   - Sanitize file paths and user input
   - Use log rotation to manage disk space

## 🔮 Future Enhancements

### Planned Features

- **Machine learning** - Anomaly detection in logs
- **Real-time analytics** - Live log analysis dashboard
- **Advanced filtering** - Complex query language for logs
- **Log correlation** - Link related log entries across operations

### Research Areas

- **Log compression** - Better compression algorithms
- **Distributed logging** - Multi-machine log aggregation
- **Log visualization** - Interactive log exploration tools
- **Predictive logging** - Anticipate issues before they occur

---

For more information, see the [main documentation](README.md) or [performance guide](PERFORMANCE.md). 