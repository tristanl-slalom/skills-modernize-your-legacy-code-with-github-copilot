# Account Management System - Test Plan

## Overview

This test plan covers the business logic and functionality of the Account Management System COBOL application. The system provides basic account operations including balance inquiry, credit transactions, and debit transactions with overdraft protection.

## Test Environment

- **Application**: Account Management System (COBOL)
- **Initial Balance**: $1,000.00
- **Test Data**: Various transaction amounts and scenarios

---

## Test Cases

| Test Case ID | Test Case Description | Pre-conditions | Test Steps | Expected Result | Actual Result | Status | Comments |
|--------------|----------------------|----------------|------------|-----------------|---------------|--------|----------|
| TC001 | View Initial Balance | System started with default balance | 1. Start application 2. Select option 1 (View Balance) | Display "Current balance: 001000.00" | | | Initial system state verification |
| TC002 | Valid Menu Navigation | Application running | 1. Enter choice 1 2. Enter choice 2 3. Enter choice 3 4. Enter choice 4 | Each option should execute corresponding function without errors | | | Basic navigation functionality |
| TC003 | Invalid Menu Choice | Application running | 1. Enter choice 5 2. Enter choice 0 3. Enter choice 9 | Display "Invalid choice, please select 1-4." for each invalid input | | | Input validation for menu choices |
| TC004 | Credit Account - Valid Amount | Current balance known | 1. Select option 2 (Credit Account) 2. Enter amount: 250.00 | Display "Enter credit amount:" prompt, Display "Amount credited. New balance: [previous + 250.00]" | | | Basic credit functionality |
| TC005 | Credit Account - Zero Amount | Current balance known | 1. Select option 2 (Credit Account) 2. Enter amount: 0.00 | Accept transaction and add 0 to balance (no change) | | | Edge case: zero credit |
| TC006 | Credit Account - Large Amount | Current balance known | 1. Select option 2 (Credit Account) 2. Enter amount: 999999.99 | Accept transaction if within system limits, display new balance | | | Boundary testing for maximum amounts |
| TC007 | Credit Account - Multiple Sequential Credits | Starting balance: 1000.00 | 1. Credit 100.00 2. Credit 50.00 3. Credit 25.00 4. View balance | Final balance should be 1175.00 | | | Multiple transaction processing |
| TC008 | Debit Account - Valid Amount (Sufficient Funds) | Current balance ≥ debit amount | 1. Select option 3 (Debit Account) 2. Enter amount: 200.00 | Display "Enter debit amount:" prompt, Display "Amount debited. New balance: [previous - 200.00]" | | | Basic debit functionality |
| TC009 | Debit Account - Insufficient Funds | Current balance < debit amount | 1. Select option 3 (Debit Account) 2. Enter amount greater than current balance | Display "Insufficient funds for this debit.", Balance remains unchanged | | | Overdraft protection |
| TC010 | Debit Account - Exact Balance Amount | Current balance = debit amount | 1. Select option 3 (Debit Account) 2. Enter amount equal to current balance | Accept transaction, new balance should be 0.00 | | | Boundary case: exact balance debit |
| TC011 | Debit Account - Zero Amount | Current balance known | 1. Select option 3 (Debit Account) 2. Enter amount: 0.00 | Accept transaction and subtract 0 from balance (no change) | | | Edge case: zero debit |
| TC012 | Debit Account - One Cent Over Balance | Current balance known | 1. Select option 3 (Debit Account) 2. Enter amount: current balance + 0.01 | Display "Insufficient funds for this debit.", Balance unchanged | | | Precise overdraft boundary testing |
| TC013 | Balance Persistence After Credit | Initial balance: 1000.00 | 1. Credit 300.00 2. View balance 3. View balance again | Both balance views should show same amount (1300.00) | | | Data persistence verification |
| TC014 | Balance Persistence After Debit | Initial balance: 1000.00 | 1. Debit 150.00 2. View balance 3. View balance again | Both balance views should show same amount (850.00) | | | Data persistence verification |
| TC015 | Mixed Transaction Sequence | Initial balance: 1000.00 | 1. Credit 500.00 (→1500.00) 2. Debit 250.00 (→1250.00) 3. Credit 100.00 (→1350.00) 4. View balance | Final balance should be 1350.00 | | | Complex transaction workflow |
| TC016 | Application Exit | Application running | 1. Select option 4 (Exit) | Display "Exiting the program. Goodbye!", Application terminates | | | Clean exit functionality |
| TC017 | Data Integrity After Failed Debit | Current balance: 500.00 | 1. Attempt debit of 600.00 2. View balance 3. Credit 100.00 4. View balance | Balance after failed debit: 500.00, Balance after credit: 600.00 | | | Failed transaction doesn't corrupt data |
| TC018 | Sequential Balance Checks | Current balance known | 1. View balance 2. View balance 3. View balance | All three displays should show identical balance | | | Consistent read operations |
| TC019 | Large Credit Transaction | Current balance: 1000.00 | 1. Credit 50000.00 2. View balance | New balance should be 51000.00 (if within system limits) | | | Large transaction handling |
| TC020 | Decimal Precision in Credits | Current balance: 1000.00 | 1. Credit 123.45 2. View balance | New balance should be 1123.45 (exact decimal precision) | | | Decimal handling accuracy |
| TC021 | Decimal Precision in Debits | Current balance: 1000.00 | 1. Debit 234.67 2. View balance | New balance should be 765.33 (exact decimal precision) | | | Decimal handling accuracy |
| TC022 | Menu Re-display After Each Operation | Application running | 1. View balance 2. Verify menu redisplays 3. Credit amount 4. Verify menu redisplays | Menu should redisplay after each operation completion | | | User interface flow |
| TC023 | Input Validation for Transaction Amounts | Application running | 1. Select credit 2. Enter negative amount 3. Select debit 4. Enter negative amount | System should handle negative inputs appropriately | | | Input validation for amounts |
| TC024 | Maximum Balance Boundary | Current balance near maximum | 1. Attempt credit that would exceed system maximum 2. View balance | System should handle overflow appropriately | | | Upper boundary testing |
| TC025 | System Reset/Restart | After multiple transactions | 1. Perform various transactions 2. Exit application 3. Restart application 4. View balance | Balance should reset to initial 1000.00 on restart | | | System initialization verification |

---

## Test Data Sets

### Standard Test Amounts
- Small amounts: 0.01, 1.00, 10.00
- Medium amounts: 100.00, 250.00, 500.00
- Large amounts: 1000.00, 5000.00, 10000.00

### Boundary Test Amounts
- Zero: 0.00
- Minimum positive: 0.01
- Current balance exactly
- Current balance + 0.01
- Maximum system value: 999999.99

### Decimal Precision Test Amounts
- 123.45, 234.67, 789.12
- 999.99, 000.01

---

## Notes for Node.js Migration

When implementing this test plan for the Node.js version:

1. **Input Validation**: Add comprehensive input validation for non-numeric inputs
2. **Async Operations**: Consider async/await patterns for data operations
3. **Error Handling**: Implement proper error handling and logging
4. **Data Persistence**: Implement proper data storage (file, database)
5. **API Testing**: Create both unit tests and integration tests
6. **Performance Testing**: Add performance benchmarks for transaction processing

## Test Execution Guidelines

1. **Prerequisites**: Ensure COBOL compiler and runtime environment are available
2. **Test Sequence**: Execute tests in order for dependency-related test cases
3. **Data Reset**: Reset system state between independent test cases
4. **Documentation**: Record all actual results and any deviations from expected results
5. **Regression Testing**: Re-run critical test cases after any code changes