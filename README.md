# looselyorganized/ci

Reusable CI workflows for Loosely Organized projects.

## Usage

Each repo calls the reusable workflow via a thin caller at `.github/workflows/ci.yml`.
This caller is managed by `/lo:new` (creates it) and `/lo:status` (updates it).
