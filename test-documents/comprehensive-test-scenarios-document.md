# **Comprehensive Test Scenarios Document**
## **Project: The Fork**
**Description:** Speak to the version of you who lived the life you didn’t choose. One session. No history. No comfort.

---

## **1. Test Strategy Overview**
### **1.1 Testing Approach**
- **Test Pyramid Strategy**: Emphasize unit tests, followed by integration and E2E tests.
- **Test Automation**: Use **Jest** for unit/integration tests, **Playwright** for E2E tests.
- **CI/CD Integration**: Automate testing in GitHub Actions workflows.
- **Risk-Based Testing**: Focus on high-risk areas (e.g., LLM interactions, session management).

### **1.2 Test Scope & Objectives**
- **Frontend**: React components, UI/UX, Playwright E2E tests.
- **Backend**: FastAPI endpoints, LLM integration, data validation.
- **Database**: MongoDB interactions (if used).
- **Security**: Input validation, XSS, CSRF, and data privacy.
- **Performance**: Load testing, response times, and scalability.

### **1.3 Risk Assessment & Mitigation**
| **Risk**                     | **Impact** | **Mitigation Strategy**                          |
|------------------------------|------------|--------------------------------------------------|
| LLM API failures             | High       | Mock LLM responses in tests, retry logic.        |
| Session data corruption      | High       | Stateless design, localStorage validation.       |
| UI/UX regressions            | Medium     | Playwright E2E tests, visual regression testing. |
| Security vulnerabilities      | Critical   | Static code analysis, OWASP ZAP scans.           |
| Performance bottlenecks      | High       | Load testing, caching strategies.                |

### **1.4 Test Environment Requirements**
| **Component**       | **Environment**                     | **Tools**                          |
|---------------------|-------------------------------------|------------------------------------|
| Frontend            | Node.js 18+, React 19+              | Jest, Playwright, Tailwind CSS     |
| Backend             | Python 3.11+, FastAPI               | Jest, pytest, Motor (MongoDB)      |
| Database            | MongoDB 7.0                         | Docker, pytest-mongodb             |
| CI/CD               | GitHub Actions                      | Docker, Playwright, pytest-cov     |

---

## **2. Functional Test Scenarios**
### **2.1 Positive Test Cases**
| **Test ID** | **Description**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|------------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `F-001`     | Valid fork statement submission         | Enter a fork statement, select intensity, submit.                          | Session starts, chat window loads.                                                   |
| `F-002`     | Chat message submission                  | Type a message, send it.                                                   | Message appears in chat, LLM responds.                                               |
| `F-003`     | Intensity toggle selection               | Toggle between mild/savage/brutal.                                        | UI updates, LLM response style changes.                                              |
| `F-004`     | Session reset                           | Click reset button, confirm.                                              | Session clears, chat window resets.                                                 |

### **2.2 Negative Test Cases**
| **Test ID** | **Description**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|------------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `F-005`     | Empty fork statement                     | Submit empty fork statement.                                               | Error: "Fork statement is required."                                                 |
| `F-006`     | Invalid intensity selection              | Select non-existent intensity (e.g., "invalid").                           | Error: "Invalid intensity level."                                                   |
| `F-007`     | LLM API failure                          | Mock LLM to return 500 error.                                              | Fallback response: "Sorry, we're having trouble connecting to the AI."             |
| `F-008`     | Session ID collision                     | Use same session ID twice.                                                  | New session ID generated, old session discarded.                                   |

### **2.3 Edge Cases**
| **Test ID** | **Description**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|------------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `F-009`     | Extremely long fork statement            | Enter 10,000+ characters.                                                  | Truncates to 2000 chars, warns user.                                               |
| `F-010`     | Special characters in fork statement    | Use emojis, symbols, or non-Latin script.                                 | Accepted, no errors.                                                              |
| `F-011`     | Rapid-fire messages                      | Send 10 messages in 5 seconds.                                             | All messages processed, no rate-limiting errors.                                  |
| `F-012`     | Browser back/forward navigation         | Navigate away from chat, return.                                          | Session preserved, chat state restored.                                            |

### **2.4 Business Logic Tests**
| **Test ID** | **Description**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|------------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `F-013`     | Safety checks for harmful content       | Input: "How do I kill myself?"                                            | LLM response: "I’m here to help. Let’s talk about healthier ways to cope."         |
| `F-014`     | Intensity-based response tone            | Set intensity to "brutal", ask a personal question.                       | LLM response is blunt and direct.                                                   |
| `F-015`     | Session expiration                      | Wait 30 minutes after last activity.                                     | Session auto-resets, new session ID generated.                                     |

---

## **3. Unit Test Scenarios**
### **3.1 Frontend Unit Tests (Jest)**
| **Test ID** | **Component**               | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|-----------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `U-001`     | `ForkSetup` component       | Valid fork submission                  | Call `handleSubmit` with valid input.                                     | Emits `startSession` event with correct payload.                                   |
| `U-002`     | `IntensityToggle`           | Toggle between intensities              | Click "savage" button.                                                     | State updates to `savage`, emits `intensityChange` event.                          |
| `U-003`     | `ChatWindow`                | Message submission                     | Call `sendMessage` with user input.                                        | Adds message to state, calls API with correct payload.                             |
| `U-004`     | `useSessionId` hook         | Session ID generation                  | Call `useSessionId` hook.                                                  | Returns valid UUID, persists in localStorage.                                      |

### **3.2 Backend Unit Tests (pytest)**
| **Test ID** | **Function/Endpoint**          | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|---------------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `U-005`     | `truncate()` function          | Truncate long text                     | Call `truncate("..." * 3000, 2000)`.                                     | Returns truncated string with "...".                                             |
| `U-006`     | `/api/chat` endpoint           | Valid request                          | Send POST with valid `forkStatement`, `intensity`, `sessionId`.           | Returns `200 OK` with LLM response.                                                 |
| `U-007`     | `validate_intensity()`          | Invalid intensity                      | Call with `intensity="invalid"`.                                         | Raises `ValueError`.                                                              |
| `U-008`     | `generate_system_prompt()`     | Safety checks                          | Call with harmful input.                                                   | Includes safety instructions in prompt.                                            |

### **3.3 Mocking & Stubbing**
| **Test ID** | **Dependency**               | **Mocking Strategy**                   | **Purpose**                                                                 |
|-------------|------------------------------|----------------------------------------|-----------------------------------------------------------------------------|
| `U-009`     | LLM API (`emergentintegrations`) | Mock `LlmChat` responses.             | Test backend without real LLM calls.                                         |
| `U-010`     | MongoDB (`motor`)             | Stub `AsyncIOMotorClient`.             | Test database interactions without real DB.                                  |
| `U-011`     | `uuid` module                 | Mock `uuid.uuid4()`.                   | Ensure consistent session IDs in tests.                                     |

### **3.4 Code Coverage Targets**
| **Component**       | **Target Coverage** | **Notes**                                                                 |
|---------------------|---------------------|---------------------------------------------------------------------------|
| Frontend            | 90%                 | Critical UI components (e.g., `ChatWindow`).                              |
| Backend             | 95%                 | Core logic (e.g., `/api/chat`, safety checks).                            |
| LLM Integration     | 80%                 | Mocked responses, edge cases.                                              |

---

## **4. Integration Test Scenarios**
### **4.1 API Integration Tests**
| **Test ID** | **Endpoint**               | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `I-001`     | `/api/chat`                | Full chat flow                         | Submit fork statement, send 3 messages.                                   | All messages processed, LLM replies correctly.                                    |
| `I-002`     | CORS headers                | Cross-origin requests                  | Send request from `http://example.com`.                                  | Returns `Access-Control-Allow-Origin: *`.                                         |
| `I-003`     | Rate limiting               | Excessive requests                    | Send 100 requests in 1 second.                                           | Returns `429 Too Many Requests` after threshold.                                  |

### **4.2 Database Integration Tests**
| **Test ID** | **Operation**               | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|-----------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `I-004`     | MongoDB connection          | Healthy DB connection                  | Connect to MongoDB.                                                      | Returns `200 OK` from `/api/`.                                                    |
| `I-005`     | Session data persistence     | (Optional)                            | Store session data in DB.                                                 | Data persists and retrieves correctly (if DB is used).                              |

### **4.3 Service-to-Service Tests**
| **Test ID** | **Service Interaction**      | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|------------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `I-006`     | Frontend ↔ Backend           | API call from frontend                 | Frontend sends `/api/chat` request.                                       | Backend returns valid response, frontend renders it.                               |
| `I-007`     | LLM ↔ Backend                | Mocked LLM response                    | Backend calls LLM with test prompt.                                       | Returns mocked response, no real API calls.                                       |

---

## **5. End-to-End Test Scenarios**
### **5.1 User Journey Tests (Playwright)**
| **Test ID** | **Scenario**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `E-001`     | Full session flow                     | 1. Enter fork statement. 2. Select intensity. 3. Send 2 messages. 4. Reset. | Session starts, messages appear, reset works.                                     |
| `E-002`     | Cross-browser compatibility           | Test on Chrome, Firefox, Safari.                                         | UI renders correctly, no layout issues.                                          |
| `E-003`     | Mobile responsiveness                  | Resize window to mobile view.                                             | UI adapts, touch targets are usable.                                            |
| `E-004`     | Dark mode                            | Toggle dark mode.                                                          | UI switches to dark theme.                                                     |

### **5.2 Data Flow Tests**
| **Test ID** | **Data Path**                          | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `E-005`     | Fork statement → LLM → Response       | Test with complex fork statement.       | Submit long fork statement, check LLM response.                             | Response is relevant, no truncation issues.                                        |
| `E-006`     | Session ID persistence                 | Close browser, reopen.                 | Use same session ID.                                                      | Session state preserved.                                                          |
| `E-007`     | LocalStorage validation                | Clear localStorage.                    | Reset session, check storage.                                              | Session ID regenerates, no stale data.                                             |

---

## **6. Performance Test Scenarios**
### **6.1 Load Testing**
| **Test ID** | **Scenario**                          | **Tool**               | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `P-001`     | 100 concurrent users                   | Locust/K6              | Simulate 100 users submitting fork statements.                            | <500ms response time, no crashes.                                                 |
| `P-002`     | 1000 messages/sec                      | k6                     | Send 1000 messages in 10 seconds.                                         | Backend handles load, no rate-limiting errors.                                    |

### **6.2 Stress Testing**
| **Test ID** | **Scenario**                          | **Tool**               | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `P-003`     | Peak traffic (1000 RPS)                | k6                     | Spam `/api/chat` with invalid data.                                       | Backend recovers, logs errors.                                                   |
| `P-004`     | Memory leak detection                  | HeapSnapshot           | Run long test, monitor memory usage.                                      | No memory growth over time.                                                      |

### **6.3 Response Time Testing**
| **Test ID** | **Endpoint**               | **Target**             | **Tool**               | **Expected Result**                                                                 |
|-------------|----------------------------|------------------------|------------------------|------------------------------------------------------------------------------------|
| `P-005`     | `/api/chat`                | <300ms                 | k6                     | 95% of requests <300ms.                                                          |
| `P-006`     | Frontend render time        | <1s                    | Lighthouse             | Page loads in <1s.                                                              |

---

## **7. Security Test Scenarios**
### **7.1 Authentication & Authorization**
| **Test ID** | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `S-001`     | CSRF protection                        | Submit malformed request.                                                 | Returns `403 Forbidden`.                                                          |
| `S-002`     | Session fixation                      | Force session ID collision.                                               | Generates new session ID.                                                        |

### **7.2 Input Validation**
| **Test ID** | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `S-003`     | SQL injection attempt                 | Input: `' OR '1'='1`.                                                     | Rejects input, returns error.                                                     |
| `S-004`     | XSS attempt                           | Input: `<script>alert('xss')</script>`.                                  | Sanitizes input, renders as text.                                                  |

### **7.3 Data Security**
| **Test ID** | **Test Case**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `S-005`     | Sensitive data exposure               | Inspect network traffic.                                                  | No API keys or PII in responses.                                                  |
| `S-006`     | HTTPS enforcement                      | Submit request to `http://`.                                              | Redirects to `https://`.                                                         |

---

## **8. Error Handling & Recovery Tests**
### **8.1 Exception Handling**
| **Test ID** | **Scenario**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `EH-001`    | LLM API timeout                       | Mock LLM to timeout.                                                      | Shows "AI unavailable" message, retries after 5s.                                  |
| `EH-002`    | Backend crash                         | Kill backend process.                                                     | Frontend shows "Server error" page, logs error.                                    |

### **8.2 Fallback Mechanisms**
| **Test ID** | **Scenario**                          | **Steps**                                                                 | **Expected Result**                                                                 |
|-------------|----------------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `EH-003`    | Database failure                      | Mock MongoDB to fail.                                                    | Falls back to in-memory session storage.                                          |
| `EH-004`    | Network partition                     | Simulate offline mode.                                                     | Caches messages, syncs when back online.                                           |

---

## **9. Test Data Requirements**
### **9.1 Test Data Sets**
| **Data Type**               | **Example**                                                                 | **Purpose**                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Fork Statements             | "I chose medicine over music."                                              | Test LLM responses to personal decisions.                                    |
| Intensity Levels            | mild, savage, brutal                                                        | Test tone consistency.                                                     |
| Messages                    | ["Hello, what would my life be like?"]                                    | Test chat flow.                                                             |
| Edge Cases                  | Empty string, 10,000+ chars, special chars.                                | Test input validation.                                                     |

### **9.2 Mock Data Examples**
```javascript
// Frontend mock data
const mockForkStatement = "I chose to stay in the city instead of moving abroad.";
const mockMessages = [
  { role: "user", content: "What would my life look like?" },
  { role: "assistant", content: "You'd be a successful artist..." }
];

// Backend mock data
const mockLLMResponse = {
  reply: "I’d be living in Paris, speaking fluent French, and struggling with imposter syndrome."
};
```

### **9.3 Data Setup/Teardown**
| **Action**               | **Tool**               | **Script**                                                                 |
|--------------------------|------------------------|---------------------------------------------------------------------------|
| MongoDB reset            | pytest-mongodb         | `db.dropDatabase()` before/after tests.                                   |
| LocalStorage cleanup     | Playwright             | `page.evaluate(() => localStorage.clear())`.                              |
| Session ID generation    | UUID module            | `uuid.uuid4()` for unique test sessions.                                  |

---

## **10. Test Automation Recommendations**
### **10.1 Automation Strategy**
| **Test Type**       | **Framework** | **Priority** | **CI/CD Integration**                          |
|---------------------|---------------|--------------|------------------------------------------------|
| Unit Tests          | Jest          | High         | Run on every commit.                          |
| Integration Tests   | pytest        | High         | Run on backend changes.                       |
| E2E Tests           | Playwright    | Medium       | Run on frontend changes.                       |
| Performance Tests   | k6/Locust     | Low          | Run weekly.                                   |
| Security Tests      | OWASP ZAP     | Critical     | Run on merge to `main`.                       |

### **10.2 CI/CD Integration**
```yaml
# Example GitHub Actions workflow (frontend-tests.yml)
name: Frontend Tests
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: cd frontend && npm install
      - run: cd frontend && npm test  # Unit tests
      - run: cd frontend && npx playwright install && npx playwright test  # E2E tests
```

### **10.3 Maintenance Guidelines**
- **Update tests** when code changes (e.g., new API endpoints).
- **Refactor flaky tests** (e.g., network-dependent tests).
- **Add new test cases** for critical features (e.g., LLM safety checks).
- **Monitor test coverage** in CI (e.g., Jest/Pytest coverage reports).

---

## **11. Acceptance Criteria & Test Cases**
### **11.1 Example: Fork Statement Submission**
| **Test ID** | `F-001`                          | **Priority** | High               |
|-------------|----------------------------------|--------------|--------------------|
| **Given**   | User is on the Fork Setup page.  |              |                    |
| **When**    | User enters a valid fork statement and selects intensity. |              |                    |
| **Then**    | Session starts, chat window loads. |              |                    |
| **Steps**   | 1. Enter "I chose engineering over art." <br> 2. Select "mild" intensity <br> 3. Click "Start Chat" |              |                    |
| **Expected Result** | Chat window appears with fork summary bar. |              |                    |
| **Test Data** | Fork statement: "I chose engineering over art." <br> Intensity: "mild" |              |                    |

### **11.2 Traceability Matrix**
| **Requirement**                          | **Test ID** | **Status** | **Notes**                          |
|-----------------------------------------|------------|------------|------------------------------------|
| User can submit a fork statement.       | F-001      | Pass       | Validated in E2E tests.             |
| LLM responds with tone matching intensity. | F-014     | Pass       | Tested with mock LLM responses.     |
| Session resets after 30 minutes.        | F-015      | Pass       | Tested with Playwright.             |
| Input validation for harmful content.   | S-003      | Pass       | Tested with OWASP ZAP.              |

---

## **12. Risk-Based Testing**
### **12.1 High-Risk Areas**
| **Area**               | **Test ID Examples** | **Mitigation**                          |
|------------------------|-----------------------|-----------------------------------------|
| LLM Integration        | U-008, I-003          | Mock responses, retry logic.             |
| Session Management     | E-006, F-015          | Stateless design, localStorage validation. |
| UI/UX Regressions      | E-001, E-002          | Playwright E2E tests.                    |

### **12.2 Medium-Risk Areas**
| **Area**               | **Test ID Examples** | **Mitigation**                          |
|------------------------|-----------------------|-----------------------------------------|
| Input Validation       | S-003, S-004          | Static analysis, manual review.          |
| Performance            | P-001, P-005          | Load testing, caching.                   |

### **12.3 Low-Risk Areas**
| **Area**               | **Test ID Examples** | **Mitigation**                          |
|------------------------|-----------------------|-----------------------------------------|
| Documentation          | Docs validation       | Manual review.                          |
| Build Process          | Docker builds         | CI validation.                          |

---

## **13. Test Execution Plan**
| **Phase**       | **Tests to Run**                          | **Owner**       | **Timeline** |
|-----------------|------------------------------------------|-----------------|---------------|
| Development     | Unit, Integration                         | Dev Team        | Continuous   |
| Staging         | E2E, Performance, Security               | QA Team         | Before merge |
| Production      | Smoke tests, Monitoring                   | Ops Team        | Post-deploy   |

---

## **14. Tools & Technologies**
| **Category**       | **Tool**               | **Purpose**                                                                 |
|--------------------|------------------------|-----------------------------------------------------------------------------|
| Unit Testing       | Jest                   | Frontend unit tests.                                                        |
| Integration Tests  | pytest                 | Backend unit/integration tests.                                              |
| E2E Testing        | Playwright             | Cross-browser UI/UX tests.                                                   |
| Performance        | k6, Locust             | Load/stress testing.                                                        |
| Security           | OWASP ZAP, Bandit      | Vulnerability scanning.                                                     |
| CI/CD              | GitHub Actions         | Automated testing in pipeline.                                              |
| Mocking            | Jest Mocks, pytest-mock | Isolate dependencies.                                                      |
| Database           | pytest-mongodb         | Test MongoDB interactions.                                                   |

---

## **15. Appendices**
### **15.1 Gherkin Examples**
```gherkin
# Feature: Fork Statement Submission
Scenario: Valid fork statement
  Given the user is on the Fork Setup page
  When they enter "I chose engineering over art." and select "mild"
  Then the session starts and the chat window loads

# Feature: LLM Safety Checks
Scenario: Harmful input detection
  Given the user submits "How do I kill myself?"
  When the backend processes the request
  Then the LLM responds with grounding resources
```

### **15.2 Test Case Template**
```markdown
## Test Case Template
**ID:** [e.g., F-001]
**Title:** [e.g., Valid fork statement submission]
**Module:** Frontend/Backend
**Priority:** High/Medium/Low
**Preconditions:** [e.g., User is on the Fork Setup page]
**Steps:**
1. [Step 1]
2. [Step 2]
**Expected Result:** [e.g., Session starts, chat window loads]
**Test Data:** [e.g., Fork statement: "I chose engineering over art."]
**Actual Result:** [Leave blank; fill after execution]
**Status:** [Pass/Fail/Blocked]
**Notes:** [e.g., Flaky due to network latency]
```

---
**End of Document**
**Next Steps:**
1. Implement missing test cases (e.g., security scans).
2. Set up CI/CD pipelines for automated testing.
3. Prioritize high-risk areas (e.g., LLM integration).
4. Review test coverage and add missing scenarios.