# CLAUDE.md - PgSync Project Information

## Project Overview

**PgSync** is a command-line tool for syncing data from one PostgreSQL database to another, similar to `pg_dump`/`pg_restore` but with enhanced capabilities.

## Key Information

- **Project Name**: pgsync
- **Type**: Ruby gem / Command-line tool
- **Author**: Andrew Kane
- **Email**: andrew@ankane.org
- **License**: MIT
- **Homepage**: https://github.com/ankane/pgsync
- **Battle-tested at**: Instacart

## Core Features

1. **Speed** - Tables are transferred in parallel for faster syncing
2. **Security** - Built-in methods to prevent sensitive data from ever leaving the server
3. **Flexibility** - Gracefully handles schema differences (missing columns, extra columns)
4. **Convenience** - Sync partial tables, groups of tables, and related records

## Technology Stack

- **Language**: Ruby (>= 2.7)
- **Database**: PostgreSQL
- **Key Dependencies**:
  - `pg` - PostgreSQL adapter
  - `parallel` - Parallel processing
  - `slop` - Command-line option parsing
  - `tty-spinner` - Terminal spinner
  - `bigdecimal` - Arbitrary-precision decimal arithmetic

## Project Structure

```
pgsync/
├── exe/               # Executable files
├── lib/               # Ruby source files
│   └── pgsync/       # Main library code
├── test/             # Test suite
├── .github/          # GitHub workflows
├── config.yml        # Default configuration
├── Dockerfile        # Docker configuration
└── README.md         # Main documentation
```

## Common Development Tasks

### Installation
```sh
gem install pgsync
# or
brew install pgsync
```

### Basic Usage
```sh
# Initialize configuration
pgsync --init

# Sync all tables
pgsync

# Sync specific tables
pgsync table1,table2

# Sync with conditions
pgsync products "where store_id = 1"
```

### Development Setup
```sh
git clone https://github.com/ankane/pgsync.git
cd pgsync
bundle install

# Create test databases
createdb pgsync_test1
createdb pgsync_test2
createdb pgsync_test3

# Run tests
bundle exec rake test
```

## Important Concepts

### Data Security
- Define data rules in `.pgsync.yml` to prevent sensitive data from leaving the server
- Support for data masking: `unique_email`, `random_letter`, `random_date`, etc.

### Configuration
- Configuration file: `.pgsync.yml`
- Supports multiple database configurations: `.pgsync-db2.yml`, etc.
- Excludes framework-specific tables (Django migrations, Rails schema_migrations, etc.)

### Safety Features
- Destination limited to `localhost`/`127.0.0.1` by default
- Requires `to_safe: true` for remote destinations
- Connection security warnings for network transfers

## Framework Integrations

- **Django**: Auto-excludes `django_migrations`
- **Rails**: Auto-excludes `ar_internal_metadata`, `schema_migrations`
- **Laravel**: Auto-excludes `migrations`
- **Heroku**: Auto-configures `from` database

## Related Projects by Author

- **Dexter**: Automatic indexer for Postgres
- **PgHero**: Performance dashboard for Postgres
- **pgslice**: Postgres partitioning tool

## Testing

The project uses a test suite with multiple PostgreSQL databases for testing synchronization features.

## Contributing

Contributions are welcome! Areas to contribute:
- Bug reports and fixes
- Documentation improvements
- New features
- Test coverage

## Version Information

Current branch work is based on recent commits including:
- Schema loading improvements
- PostgreSQL 18 testing
- Various bug fixes and enhancements

## Notes for AI Assistants

- This is a data synchronization tool, not an ORM or migration framework
- Focus is on reliability, security, and performance
- Production use requires careful configuration of `.pgsync.yml`
- Security is paramount - always handle sensitive data appropriately
- Foreign key handling requires special attention (defer constraints recommended)
