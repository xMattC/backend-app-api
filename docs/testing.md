# Testing

This project uses automated tests to verify business behaviour, authentication, ownership rules, relationship handling, validation, and API behaviour across the Workout API platform.

The test suite is organised into two practical layers:

- Unit tests
- API / System tests

---

## Unit Tests

Unit tests validate isolated application logic.

Examples include:

- Model methods and helper functions
- Serializer validation logic
- Data transformation behaviour
- Derived calculations
- Business rules
- Validation behaviour

These tests ensure internal logic behaves correctly in isolation.

---

## API / System Tests

API tests validate complete request–response behaviour.

This includes:

- Authentication workflows
- Permissions and ownership enforcement
- Database-backed API behaviour
- API request and response validation
- Nested relationship handling
- File upload behaviour

These tests simulate realistic client interaction through HTTP requests.

---

# Architectural Guarantees

The following guarantees are enforced through application logic and validated through automated tests.

## Authentication

- Private endpoints require authentication
- Unauthenticated users cannot access protected resources
- Authenticated users can access their own resources
- Authentication state is enforced consistently across API endpoints

---

## User Isolation

- Users can only access their own workouts
- Users cannot retrieve another user’s workout
- Users cannot update another user’s workout
- Users cannot delete another user’s workout

- Users can only access their own tags
- Users cannot modify another user’s tags

- Exercises and related entities remain scoped to the authenticated user where applicable

---

## Workout and Relationship Behaviour

- Workouts can be created, updated, and deleted by their owner
- Workout ownership cannot be reassigned through API requests
- Nested relationships are handled correctly during create and update operations

### Tags

- New tags are created when required
- Existing tags are reused where possible
- Updating tags replaces previous assignments
- Empty lists remove all associated tags

### Exercises

- New exercises are created when required
- Existing exercises are reused where possible
- Updating exercises replaces previous assignments
- Empty lists remove all associated exercises

---

## Validation and Data Integrity

- Invalid payloads are rejected
- Required fields must be provided
- Invalid relationship data is not persisted
- Ownership rules remain enforced
- Restricted fields cannot be modified through requests

---

## API Guarantees

- List endpoints return only authenticated user data
- Detail endpoints return complete resource representations
- Nested relationships are correctly serialized
- API responses remain consistent with serializer definitions
- Validation failures return appropriate HTTP status codes

---

## Media Upload Handling

- Valid image uploads are accepted and stored correctly
- Uploaded images are associated with the correct workout
- Invalid uploads return appropriate error responses
- File storage state reflects successful uploads

---

# Continuous Verification

The project uses continuous verification during development.

This includes:

- Automated local test execution
- Django test runner execution
- CI pipeline execution
- Validation of API behaviour and business rules

---

# Test File Reference

## Unit Tests

Admin:

- [`test_admin.py`](../app/core/tests/test_admin.py)

Core:

- [`test_commands.py`](../app/core/tests/test_commands.py)
- [`test_models.py`](../app/core/tests/test_models.py)


## API / System Tests

User API:

- [`test_user_api.py`](../app/user/tests/test_user_api.py)

Workout API:

- [`test_exercise_api.py`](../app/workout/tests/test_exercise_api.py)
- [`test_exercise_tags_api.py`](../app/workout/tests/test_exercise_tags_api.py)
- [`test_workout_api.py`](../app/workout/tests/test_workout_api.py)
- [`test_workout_tags_api.py`](../app/workout/tests/test_workout_tags_api.py)

---

# Summary

The automated test suite provides confidence that the platform:

- Enforces strict user-level isolation
- Preserves ownership boundaries
- Correctly manages workout relationships
- Rejects invalid or inconsistent data
- Maintains API consistency
- Safely handles media uploads
- Remains maintainable as the application evolves
