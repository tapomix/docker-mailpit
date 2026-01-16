<!-- markdownlint-disable MD024 -->
# Changelog

All notable changes to this project will be documented in this file.

---

## [0.1.0] - 2026-01-17

### Added

- Docker Compose configuration with Traefik integration
- Security hardening (rootless, read-only, no-new-privileges, cap_drop)
- Persistent SQLite storage via mounted volume
- Healthcheck using Mailpit API
- Environment variables template (`.env.dist`)
- Documentation [README](README.md)

---

## About

This project follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).  
The changelog format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
