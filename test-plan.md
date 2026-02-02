# Test Plan - Borsibaar (Bar POS with Dynamic Pricing)

## 1. Testing Objectives

- Verify dynamic pricing algorithm adjusts prices correctly based on demand
- Ensure accurate sales transactions and inventory management
- Validate authentication and authorization (bar staff and managers)
- Confirm real-time price display updates on customer screens
- Test concurrent transaction handling during peak usage
- Verify system performance under realistic load

## 2. Testing Levels

### 2.1 Unit Testing
**Scope:** Individual components in isolation with mocked dependencies

**Backend (JUnit 5 + Mockito)**
- Service layer: ProductService, SalesService, InventoryService, PriceCorrectionJob
- Security: JwtService, JwtAuthenticationFilter
- Mappers and DTOs
- Exception handlers

**Frontend (Jest + React Testing Library)**
- React components
- Utility functions
- Validation logic

**Key Tests**
- Price calculation logic with different demand levels
- Inventory deduction on sales
- JWT token generation/validation
- Exception handling

**Target:** 80% code coverage

### 2.2 Performance Testing
**Scope:** Verify system handles realistic load

**Test Scenarios**
- Normal load: 10-20 concurrent transactions
- Peak load: 30-50 concurrent transactions
- Measure response times and identify bottlenecks

**Metrics**
- API response time: < 500ms (95th percentile)
- Frontend page load: < 3s
- No transaction failures under normal load

**Tools:** JMeter or Gatling

### 2.3 Functional Testing
**Scope:** Verify features work according to requirements

**Critical Features**
- Sales: Process transactions, update inventory, calculate prices
- Dynamic Pricing: Price increases on demand, price correction job
- Inventory: Add/adjust stock, track transactions
- Products & Categories: CRUD operations
- Authentication: Login/logout, OAuth2, role-based access
- Bar Stations: Multi-station support, sales tracking
- Customer Display: Real-time price updates

**Test Approach**
- Manual testing for critical workflows
- Automated tests for stable features
- Focus on end-to-end user scenarios

### 2.4 Concurrency Testing
**Scope:** Verify correctness under simultaneous operations

**Critical Scenarios**
- Multiple bar stations selling same product simultaneously
- Inventory updates during active sales
- Price correction job running during transactions

**What to Verify**
- No overselling (inventory stays accurate)
- No race conditions or data corruption
- Transactions use correct prices
- No deadlocks

**Test Cases**
- 5-10 concurrent sales of same product
- Inventory adjustments during sales
- Price updates during active transactions

**Tools:** JMeter/Gatling, database transaction logs

## 3. Test Scope

**Backend**
- Authentication & authorization (OAuth2, JWT)
- Sales processing
- Inventory management
- Product & category management
- Dynamic pricing algorithm
- Bar station management
- Sales statistics

**Frontend**
- POS interface
- Customer display screen
- Manager dashboard
- Login/onboarding flows
- Protected routes

**Infrastructure**
- Docker setup
- PostgreSQL + Liquibase migrations
- NGINX configuration

## 4. Test Approach

**Strategy**
- Write unit tests alongside development
- Prioritize testing critical features: sales, pricing, inventory, authentication
- Automate regression tests in CI/CD
- Manual testing for complex user flows

**Tools**

*Backend:* JUnit 5, Mockito, Spring Boot Test, H2/Testcontainers, MockMvc

*Frontend:* Jest, React Testing Library, Playwright/Cypress (optional)

*Performance:* JMeter or Gatling

*CI/CD:* GitHub Actions / GitLab CI

**Test Data**
- Use Liquibase for seed data
- Generate test fixtures programmatically
- Clean up after each test run

## 5. Test Environment

**Development**
- Local: Docker Compose with PostgreSQL
- Backend: Java 21, Spring Boot, Maven
- Frontend: Next.js 15, Node.js

**Testing**
- Unit tests: H2 in-memory database
- Integration tests: Docker Compose stack
- Performance tests: Staging-like environment

**Browsers**
- Chrome, Firefox, Safari (latest versions)
- Test on desktop (1920x1080) and laptop (1366x768)

## 6. Entry and Exit Criteria

**Entry Criteria**
- Code merged to feature branch
- Passes linting and compiles
- Developer has tested locally

**Exit Criteria**

*Unit Tests:*
- 80%+ code coverage
- All tests passing
- No critical defects

*Performance Tests:*
- API response time < 500ms (95th percentile)
- System stable under peak load
- No memory leaks

*Functional Tests:*
- All critical features tested
- Major bugs resolved
- Key workflows validated

*Concurrency Tests:*
- No data corruption
- Inventory accuracy maintained
- No deadlocks

## 7. Roles and Responsibilities

**Everyone writes tests for their own code**
- Write unit tests as you develop features
- Aim for 80%+ coverage
- Check test coverage in PR reviews
- Fix your own failing tests

**Testing will happen in two modes:**

*Ongoing (as features are built):*
- Unit tests written alongside code
- Quick manual testing locally
- Automated tests run in CI/CD

*Before release (one big push):*
- Manual functional testing of all features
- Performance testing session (4-6 hours)
- Concurrency testing session (3-5 hours)
- Bug fixing sprint
- Final smoke test

**Division of work (flexible):**
- Everyone: Unit tests for their features
- 1-2 people: Set up performance tests
- 1-2 people: Set up concurrency tests
- 1 person: CI/CD pipeline configuration
- Everyone: Manual testing and bug fixes

## 8. Risks and Assumptions

**Key Risks**
- **Pricing Algorithm Bugs:** Could cause revenue loss
  - *Mitigation:* Thorough unit tests, manual validation
- **Race Conditions:** Concurrent sales causing inventory issues
  - *Mitigation:* Concurrency testing, transaction isolation
- **Auth Vulnerabilities:** Unauthorized access to manager functions
  - *Mitigation:* Security code reviews, role-based access testing
- **Display Sync:** Customer display showing stale prices
  - *Mitigation:* Real-time update testing

**Assumptions**
- PostgreSQL and Docker are available in all environments
- OAuth2 provider is reliable
- Peak load: ~30-50 concurrent transactions
- Team has basic testing knowledge (JUnit, Jest)
- Defects will be fixed within reasonable time

## 9. Test Deliverables

**Test Code**
- Unit test suites (JUnit, Jest)
- Functional test scenarios
- Performance test scripts (JMeter/Gatling)
- Concurrency test cases

**Documentation**
- Test plan (this document)
- Test coverage reports
- Bug reports in GitHub/GitLab Issues
- CI/CD pipeline results

**Reports**
- Test execution summary
- Code coverage metrics
- Performance benchmarks
- Known issues list

---

## Appendix: Time Estimates

**Per Feature/Story**
- Unit Tests: 1-2 hours
- Functional Tests: 2-4 hours
- Bug fixes: As needed

**Per Sprint/Release**
- Performance Testing: 4-6 hours
- Concurrency Testing: 3-5 hours
- Test maintenance: 2-3 hours

**Total estimated testing time per feature:** 3-6 hours
