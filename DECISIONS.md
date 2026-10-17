# Decisions log

- Initial implementation uses `Base.metadata.create_all()` at app startup for a zero-friction first run; Alembic environment is included for versioned schema changes, and production deployments should use migrations.
- The initial UI is consolidated into role-filtered Streamlit navigation to keep the first runnable release compact; service logic remains outside page code.
- Arabic font styling falls back to locally installed system fonts rather than downloading a web font, preserving offline operation.
- Work-order numbering uses yearly count-plus-one for the single-server MVP; deployments with concurrent high-volume creation should move sequence allocation into a transactional counter.
