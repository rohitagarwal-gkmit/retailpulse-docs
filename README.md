# RetailPulse Documentation

> Comprehensive documentation for RetailPulse - A web-based system for billing, inventory, and reporting for medical wholesalers.

[![Documentation](https://img.shields.io/badge/docs-mkdocs-blue)](https://www.mkdocs.org/)
[![Material Theme](https://img.shields.io/badge/theme-material-blue)](https://squidfunk.github.io/mkdocs-material/)

## 📖 About

This repository contains the complete documentation for **RetailPulse**, a system designed to streamline operations for medical wholesalers by automating:

- **Bill Generation** - Fast digital billing with automatic PDF creation
- **Inventory Management** - Real-time stock tracking with expiry dates and batch management
- **Analytics & Reporting** - Business insights and performance dashboards


## 📚 Documentation Structure

```
docs/
├── index.md                    # Home page & overview
├── functional_docs.md          # User roles & system modules
├── use_cases.md               # User workflows & scenarios
├── schema_design.md           # Database design & ERD
├── edge_cases.md              # Error handling & edge cases
├── future_enhancements.md     # Planned features
└── technical/
    ├── architecture.md        # System architecture
    ├── data_flow.md          # Data flow diagrams
    ├── auth.md               # Authentication & security
    ├── tech_stack.md         # Technology stack
    └── api_design.md         # API specifications
```

## 🎯 What's Included

### Functional Documentation
- User roles and permissions
- System modules and features
- Complete user workflows
- Use case scenarios

### Technical Documentation
- System architecture and design
- RESTful API specifications
- Authentication and authorization
- Data flow diagrams
- Technology stack details

### Database Design
- Entity Relationship Diagram (ERD)
- Complete table schemas
- Relationships and constraints

### Additional Resources
- Edge cases and error handling
- Future enhancement roadmap

## 🛠️ Built With

- [MkDocs](https://www.mkdocs.org/) - Documentation generator
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) - Material Design theme
- [mkdocs-with-pdf](https://github.com/ric-evans/mkdocs-with-pdf) - PDF export plugin
- [PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/) - Markdown extensions (including Mermaid diagrams)

## 📄 PDF Export

A PDF version of the complete documentation is automatically generated during the build process and can be found at:
```
site/pdf/document.pdf
```
