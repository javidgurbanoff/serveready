# ServeReady User Flows

## 1. Employee Onboarding

### Goal

Allow a restaurant manager to invite a new employee and allow that employee to securely join the restaurant and begin their onboarding.

### Flow

1. Manager enters employee name, email, and role.
2. Manager sends invitation.
3. ServeReady creates a unique invitation.
4. Employee receives invitation.
5. Employee opens invitation.
6. System validates invitation.
7. Employee creates an account.
8. System associates the employee with the restaurant.
9. System assigns the employee role.
10. System creates an onboarding record.
11. Employee is redirected to their dashboard.
12. Employee begins onboarding.

### Possible States

Invitation:

- Pending
- Accepted
- Expired
- Cancelled

Onboarding:

- Not Started
- In Progress
- Completed

### Failure Cases

- Invitation expired.
- Invitation already accepted.
- Email already belongs to an existing employee.
- Invalid invitation.
- Employee attempts to access another restaurant's data.

## 2. Manager Setup

### Goal

Allow a restaurant manager to configure their restaurant in ServeReady so that employees can receive restaurant-specific training.

### Flow

1. Manager signs in.
2. Manager opens the restaurant dashboard.
3. Manager creates or selects their restaurant.
4. Manager enters basic restaurant information.
5. Manager creates the restaurant floor plan.
6. Manager adds tables and assigns table numbers.
7. Manager adds restaurant areas such as dining room, bar, kitchen, and service areas.
8. Manager adds menu items.
9. Manager adds ingredients and allergen information for each menu item.
10. Manager creates or activates training modules.
11. Manager reviews the training content.
12. Manager invites new employees.
13. Invited employees become available in the manager dashboard.
14. Manager can monitor employee onboarding progress.

### Restaurant Information

The restaurant may contain:

- Name
- Address
- Description
- Logo
- Contact information
- Opening hours
- Restaurant areas

### Floor Plan

The restaurant floor plan may contain:

- Tables
- Table numbers
- Sections
- Bar
- Kitchen
- Restrooms
- Entrance
- Service areas

### Menu Information

Each menu item may contain:

- Name
- Description
- Price
- Ingredients
- Allergens
- Category
- Guest-facing description
- Service notes

### Failure Cases

- Manager attempts to create duplicate table numbers.
- Required restaurant information is missing.
- Menu item is missing required information.
- Manager attempts to access another restaurant.
- Manager attempts to invite an employee with an invalid email address.

## 3. Employee Training

### Goal

Allow a new employee to learn restaurant-specific knowledge and service procedures through short, interactive training modules.

### Flow

1. Employee opens the onboarding dashboard.
2. Employee sees all assigned training modules.
3. Employee selects an available module.
4. System displays the module introduction.
5. Employee studies the lesson content.
6. Employee completes interactive exercises.
7. System records completed lessons.
8. Employee continues through the module.
9. Employee completes the module assessment.
10. System calculates the assessment result.
11. System records the employee's score and progress.
12. Employee can review completed material.
13. Employee continues to the next available module.

### Training Modules

The initial modules are:

1. Restaurant Orientation
2. Table Knowledge
3. Menu Knowledge
4. Ingredients and Allergens
5. Service Basics
6. Guest Interaction
7. Restaurant-Specific Procedures

### Restaurant Orientation

Employees learn:

- Restaurant layout
- Entrance
- Dining areas
- Bar
- Kitchen
- Restrooms
- Service areas
- Other important locations

### Table Knowledge

Employees learn:

- Table numbers
- Table locations
- Sections
- Table capacity where relevant
- Special table information where relevant

Interactive exercises may include:

- Identify a table on the map.
- Identify a table number from its location.
- Find a table from a starting location.
- Match tables to sections.

### Menu Knowledge

Employees learn:

- Menu categories
- Menu item names
- Descriptions
- Ingredients
- Allergens
- Prices
- Important service information

### Ingredients and Allergens

Employees learn verified information about:

- Ingredients
- Allergens
- Dietary information where provided
- Possible substitutions where relevant

Employees should be instructed never to guess when answering an allergen-related question and to confirm uncertain information with the appropriate restaurant staff.

### Service Basics

Employees learn:

- Proper tray handling
- Carrying glasses
- Carrying plates
- Serving procedures
- Clearing procedures
- Table etiquette
- Restaurant-specific service standards

### Guest Interaction

Employees learn:

- Approaching guests
- Eye contact
- Body language
- Reading guest signals
- Taking orders
- Answering questions
- Handling common guest situations
- Escalating difficult situations appropriately

### Training Progress

Each employee has an individual progress record.

Example:

Restaurant Orientation: 100%
Table Knowledge: 90%
Menu Knowledge: 70%
Ingredients & Allergens: 60%
Service Basics: 30%
Guest Interaction: 0%

Overall progress should be calculated from completed training requirements rather than simply from time spent inside the application.

### Module States

Each module may have one of the following states:

- Locked
- Available
- In Progress
- Completed

### Failure Cases

- Employee attempts to access a module that has not been assigned.
- Training content cannot be loaded.
- Employee loses connection during training.
- Assessment cannot be submitted.
- Employee fails an assessment.
- Employee attempts to skip required training.

## 4. Knowledge Assessment

### Goal

Verify that an employee understands the required restaurant knowledge before completing onboarding.

### Flow

1. Employee becomes eligible for an assessment.
2. System presents a set of questions.
3. Employee answers each question.
4. System records the answers.
5. Employee submits the assessment.
6. System evaluates the answers.
7. System calculates the score.
8. System identifies incorrect answers and weak areas.
9. System records the assessment attempt.
10. If the employee passes, the module is marked as completed.
11. If the employee does not pass, the employee receives feedback and may review the relevant material.
12. Employee may attempt the assessment again according to the restaurant's configured rules.

### Question Types

The MVP may support:

- Multiple choice
- True/false
- Image identification
- Table identification
- Scenario-based questions

### Assessment Results

The employee should see:

- Score
- Pass/fail status
- Correct answers
- Areas requiring review
- Number of attempts

The manager should be able to see:

- Employee score
- Completion status
- Assessment attempts
- Weak training areas

### Failure Cases

- Employee submits incomplete assessment.
- Assessment submission fails.
- Employee loses connection while submitting.
- Assessment contains invalid or unavailable questions.

## 5. Manager Progress Monitoring

### Goal

Give managers visibility into employee onboarding without requiring them to manually track training progress.

### Flow

1. Manager opens the restaurant dashboard.
2. Manager views employees currently undergoing onboarding.
3. Manager sees each employee's overall progress.
4. Manager selects an employee.
5. System displays detailed training progress.
6. Manager can identify incomplete or weak areas.
7. Manager can review assessment results.
8. Manager can determine whether additional training or supervision is required.

### Employee Progress Information

The manager may see:

- Employee name
- Role
- Overall progress
- Completed modules
- In-progress modules
- Assessment scores
- Number of attempts
- Last activity
- Training areas requiring attention

### Example

Employee: Javid Gurbanov

Overall: 84%

Restaurant Orientation: 100%
Table Knowledge: 95%
Menu Knowledge: 88%
Ingredients & Allergens: 82%
Service Basics: 76%
Guest Interaction: 71%

Status: In Progress

### Manager Actions

A manager may:

- View employee progress
- Review assessment results
- Assign training
- Remove training
- Invite employees
- Manage restaurant content
