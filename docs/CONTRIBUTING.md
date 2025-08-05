# Contributing to Riceify

Thank you for your interest in contributing to Riceify! This document provides guidelines and information for contributors.

## 🤝 How to Contribute

### Types of Contributions

We welcome various types of contributions:

- **Bug reports** - Report issues and bugs
- **Feature requests** - Suggest new features
- **Code contributions** - Submit code improvements
- **Documentation** - Improve or add documentation
- **Testing** - Help test and validate features
- **Community support** - Help other users

### Getting Started

1. **Fork the repository**
   ```bash
   git clone https://github.com/MadeInLithuania/Riceify.git
   cd Riceify
   ```

2. **Set up development environment**
   ```bash
   mkdir build && cd build
   cmake -DCMAKE_BUILD_TYPE=Debug ..
   make -j$(nproc)
   ```

3. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## 🐛 Reporting Bugs

### Before Reporting

1. **Check existing issues** - Search for similar issues
2. **Reproduce the bug** - Ensure it's reproducible
3. **Check documentation** - Verify it's not a configuration issue

### Bug Report Template

```markdown
**Bug Description**
Brief description of the bug.

**Steps to Reproduce**
1. Step 1
2. Step 2
3. Step 3

**Expected Behavior**
What should happen.

**Actual Behavior**
What actually happens.

**Environment**
- OS: [e.g., Ubuntu 22.04]
- Riceify Version: [e.g., 1.2.3]
- Compiler: [e.g., GCC 11.2]
- CMake Version: [e.g., 3.22.1]

**Additional Information**
- Logs: [Attach relevant log files]
- Screenshots: [If applicable]
- Configuration: [Share relevant config]
```

## 💡 Feature Requests

### Feature Request Template

```markdown
**Feature Description**
Brief description of the requested feature.

**Use Case**
Why this feature would be useful.

**Proposed Implementation**
How you think it could be implemented.

**Alternatives Considered**
Other approaches you've considered.

**Additional Information**
Any other relevant details.
```

## 💻 Code Contributions

### Development Setup

#### Prerequisites

- **C++17** compatible compiler (GCC 8+, Clang 7+, MSVC 2019+)
- **CMake** 3.16 or higher
- **Git** for version control
- **Make** or **Ninja** for building

#### Build Dependencies

```bash
# Ubuntu/Debian
sudo apt install build-essential cmake git libboost-all-dev

# Fedora
sudo dnf install gcc-c++ cmake git boost-devel

# Arch Linux
sudo pacman -S base-devel cmake git boost

# macOS
brew install cmake boost
```

#### Development Build

```bash
# Clone and setup
git clone https://github.com/MadeInLithuania/Riceify.git
cd Riceify

# Configure with debug options
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Debug \
      -DENABLE_TESTS=ON \
      -DENABLE_COVERAGE=ON \
      ..

# Build
make -j$(nproc)

# Run tests
make test
```

### Code Style Guidelines

#### C++ Style

Follow the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) with these modifications:

```cpp
// File naming: snake_case.cpp
// Class naming: PascalCase
class RiceManager {
private:
    // Member variables: snake_case with trailing underscore
    std::string config_path_;
    bool is_initialized_;
    
public:
    // Method naming: snake_case
    bool initialize_system();
    void save_rice_config(const std::string& rice_name);
    
    // Constants: kPascalCase
    static constexpr int kMaxRetries = 3;
};

// Function naming: snake_case
void process_config_file(const std::string& file_path) {
    // Local variables: snake_case
    std::string file_content;
    bool is_valid = false;
    
    // Use meaningful variable names
    if (file_content.empty()) {
        LOG_ERROR("Empty file content");
        return;
    }
}
```

#### Formatting

Use `clang-format` for consistent formatting:

```bash
# Install clang-format
sudo apt install clang-format

# Format code
clang-format -i src/*.cpp include/*.h

# Check formatting
clang-format --dry-run --Werror src/*.cpp
```

#### Documentation

TODO

### Testing Guidelines

TODO

#### Unit Tests

Write unit tests for all new functionality:

```cpp
#include <gtest/gtest.h>
#include "rice_manager.h"

class RiceManagerTest : public ::testing::Test {
protected:
    void SetUp() override {
        manager_ = std::make_unique<RiceManager>();
    }
    
    void TearDown() override {
        // Cleanup test files
    }
    
    std::unique_ptr<RiceManager> manager_;
};

TEST_F(RiceManagerTest, SaveRiceConfig_ValidName_ReturnsTrue) {
    // Arrange
    std::string rice_name = "test_rice";
    
    // Act
    bool result = manager_->save_rice_config(rice_name);
    
    // Assert
    EXPECT_TRUE(result);
}
```

#### Integration Tests

Test complete workflows:

```cpp
TEST_F(RiceManagerIntegrationTest, CompleteRiceWorkflow) {
    // Test complete rice save -> apply -> restore workflow
    EXPECT_TRUE(manager_->save_rice_config("test_rice"));
    EXPECT_TRUE(manager_->apply_rice_config("test_rice"));
    EXPECT_TRUE(manager_->restore_previous_config());
}
```

#### Performance Tests

Benchmark critical operations:

```cpp
TEST_F(RiceManagerPerformanceTest, LargeConfigBackup) {
    // Create large configuration
    create_large_config();
    
    auto start = std::chrono::high_resolution_clock::now();
    bool result = manager_->save_rice_config("large_rice");
    auto end = std::chrono::high_resolution_clock::now();
    
    auto duration = std::chrono::duration_cast<std::chrono::milliseconds>(end - start);
    EXPECT_TRUE(result);
    EXPECT_LT(duration.count(), 1000); // Should complete in < 1 second
}
```

### Commit Guidelines

#### Commit Message Format

Use conventional commit format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(backup): add incremental backup support

fix(cache): resolve memory leak in cache manager

docs(readme): update installation instructions

test(performance): add benchmarks for rice switching
```

#### Commit Guidelines

1. **One change per commit** - Keep commits focused
2. **Write clear messages** - Explain what and why, not how
3. **Reference issues** - Link to related issues
4. **Test before committing** - Ensure all tests pass

### Pull Request Process

#### Before Submitting

1. **Update documentation** - Update relevant docs
2. **Add tests** - Include tests for new features
3. **Update changelog** - Document changes
4. **Self-review** - Review your own code

## 🔍 Code Review Process

### Review Guidelines

1. **Be constructive** - Provide helpful feedback
2. **Focus on code** - Avoid personal comments
3. **Ask questions** - Clarify unclear code
4. **Suggest improvements** - Offer alternatives

### Review Checklist

- [ ] Code follows style guidelines
- [ ] Tests are included and pass
- [ ] Documentation is updated
- [ ] Performance impact is considered
- [ ] Security implications are addressed
- [ ] Error handling is appropriate

## 🚀 Release Process

## 🏆 Recognition

### Contributors

We recognize contributors in several ways:

1. **Contributor list** - Listed in README.md
2. **Release notes** - Mentioned in release announcements
3. **GitHub stars** - Star the repository
4. **Community badges** - Special badges for contributors

### Hall of Fame

Special recognition for significant contributions:

- **Core Contributors** - Major code contributions
- **Documentation Heroes** - Outstanding documentation work
- **Bug Hunters** - Exceptional bug reporting and fixing
- **Community Champions** - Outstanding community support

## 📞 Getting Help

### Communication Channels

- **GitHub Issues** - Bug reports and feature requests
- **GitHub Discussions** - General questions and discussions
- **Pull Requests** - Code contributions and reviews
- **Email** - Private or sensitive matters

### Resources

- [GitHub Repository](https://github.com/MadeInLithuania/Riceify)
- [Issue Tracker](https://github.com/MadeInLithuania/Riceify/issues)
- [Discussions](https://github.com/MadeInLithuania/Riceify/discussions)
- [Documentation](https://github.com/MadeInLithuania/Riceify/tree/main/docs)

## 📄 License

By contributing to Riceify, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to Riceify! Your contributions help make this project better for everyone. 