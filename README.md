# Spectest Documentation

This repository contains the official documentation for [Spectest](https://github.com/justiceo/spectest), a lightning-fast and declarative API testing tool.

## About Spectest

Spectest is an open-source CLI tool for declarative HTTP API testing that allows you to write tests in JavaScript or JSON without boilerplate code. It's designed to be fast, focused, and developer-friendly.

### Key Features

- ⚡ **Fast Execution**: Execute hundreds of tests in seconds with parallel execution
- 📝 **Truly Declarative**: Write tests in JavaScript or JSON - no boilerplate, just describe what you expect
- 🎯 **Focused Workflow**: Built specifically for API testing with smart defaults and intuitive syntax
- 🔧 **Easy Integration**: Works with existing development workflows and CI/CD pipelines

## Documentation Site

This repository contains the documentation site built with [Hugo](https://gohugo.io/) using the [Hextra](https://github.com/imfing/hextra) theme. The site provides comprehensive guides, API references, and examples for using Spectest.

### Live Site

The documentation is available at: [https://dainty-chimera-67d839.netlify.app/](https://dainty-chimera-67d839.netlify.app/)

## Local Development

### Prerequisites

- [Go](https://golang.org/dl/)
- [Hugo](https://gohugo.io/installation/) 

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Neche-Stephen/spec-test
   cd spec-test
   ```

2. **Start the development server**
   ```bash
   hugo server
   ```

3. **Open your browser**
   Navigate to `http://localhost:1313` to view the documentation site.

### Project Structure

```
spectest-docs/
├── content/                    # Documentation content (Markdown files)
│   ├── _index.md              # Homepage content
│   └── docs/                  # Documentation sections
│       ├── introduction/       # Getting started guides
│       ├── api-references/     # CLI and API documentation
│       ├── guides/            # How-to guides
│       ├── integrations/      # Framework integrations
│       └── more/              # About, terms, etc.
├── layouts/                   # shortcodes
├── static/                    # Static assets (favicons)
├── assets/                    # Source assets (CSS)
├── hugo.toml                  # Hugo configuration
└── go.mod                     # Go module dependencies
```

## Building for Production

To build the static site for production:

```bash
hugo --minify
```

The built site will be in the `public/` directory, ready for deployment.

**Note**: This repository contains only the documentation site. For the actual Spectest testing tool, visit [https://github.com/justiceo/spectest](https://github.com/justiceo/spectest). 