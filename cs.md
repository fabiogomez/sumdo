# Project Constitution: GloberBooks
## Tech Stack
- : [Your language [framework: Node . js + Express /
Python + FastAPI]
- Postgresql 
**Frontend:** [Your framework: React 
## Architecture Principles
Layered architecture: Controllers — Services — Data Access
- Dependency injection where applicable
- Single Responsibility Principle for all classes/modules
## Boundaries
ALWAYS Do:
- Write unit tests for all business logic before finalizing a
task
- Follow RESTful API naming conventions (use /api/vl / • prefix)
- Use explicit error handling and return standard HTTP status
codes
- Add comments for all public interfaces
- Use async/await (not callbacks) for asynchronous operations
- Validate all user inputs
ASK FIRST:
- Before adding any new third-party dependencies/ libraries
- Before modifying the database schema
- Before adding new entities/ tables
- Before changing architectural patterns
S NEVER DO:
commit secrets
expose credentials
- Never
Code
hardcode secrets, API keys, or sensitive data in code
skip writing tests for business logic
use SELECT * in production code
store passwords in plain text
add features not in spec . md
Style
- Naming: camelCase for variables/ functions,
classes
Max function length: 50 lines
- Max file length: 30B lines
PascalCase for
- Descriptive variable names (minimum 3 characters)
## Error Handling Standards
All errors must return:
"error" .
• "Human-readable message" ,
"code : '
"timestamp"
. " ISO-8681 "
Status
- 2Be:
codes :
Success with data
- 201 :
- 400 :
- 404:
- 50e:
Resource created
Validation errors (return field-specific messages)
Resource not found
Unexpected server errors
## Testing Requirements
- Minimum 80% code coverage
- Test file naming :
[filename] . test. [ext]
[filename) . spec. [extl
- Use AAA pattern: Arrange, Act, Assert
- Mock external dependencies (DB, APIs)
## API Standards
- All endpoints prefixed with • /api/vl / •
RESTful conventions (GET/POST/PUT/DELETE)
- Consistent response format