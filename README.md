# Repoint-R

Repoint-R is a desktop application for browsing and managing Documentum repositories. It provides a graphical interface for executing DQL queries, browsing the repository structure, viewing object properties, and performing common operations like check-in/check-out.

## Features

- **Repository Browser** - Navigate cabinets, folders and documents
- **DQL Query Editor** - Execute queries with results viewer
- **Object Properties** - View and edit object attributes
- **ACL Viewer** - Inspect access control lists
- **Check-in/Check-out** - Content management operations
- **External Docbroker** - Connect to docbrokers not in dfc.properties

## Requirements

- Java 17 or later
- Documentum DFC libraries (from your Content Server or client installation)
- dfc.properties configured for your environment (optional - can add docbrokers at runtime)

## Quick Start

See [BUILD.md](BUILD.md) for detailed build and installation instructions.

```bash
# Build
mvn clean package

# Run (after extracting and adding DFC JARs)
./repoint
```

## Screenshots

*Coming soon*

## History

This is a modernised fork of the original Repoint application:

- Upgraded to **Eclipse 2023-06** platform
- Upgraded to **Tycho 4.0.4** build system
- Updated to **Java 17** runtime
- Fixed deprecated Eclipse API usage

## License

Eclipse Public License v1.0

## Credits

Original Repoint application by EMC Developer Network (2006).
