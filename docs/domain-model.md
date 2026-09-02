# ServeReady Domain Model

## 1. Users

A User represents a person who has an account on ServeReady.

### User properties

- id
- name
- email
- password/authentication reference
- created_at

### Rules

- A user belongs to one restaurant in the MVP.
- A user has one primary role.
- Roles in the MVP are employee and manager.

---

## 2. Restaurants

A Restaurant represents a hospitality business using ServeReady.

### Restaurant properties

- id
- name
- address
- logo
- contact information
- created_at

### Rules

- Each restaurant has its own data.
- Restaurant data must not be accessible to users from another restaurant.
- A restaurant has employees and managers.
- Restaurant-specific training, menu information, tables, and floor-plan data are private to that restaurant.

---

## 3. Memberships

A Membership represents a user's relationship with a restaurant.

### Membership properties

- id
- user_id
- restaurant_id
- role
- created_at

### Roles

- employee
- manager

### Rules

- A user belongs to one restaurant in the MVP.
- A user has one primary role.
- A manager manages one restaurant in the MVP.
- An employee works for one restaurant in the MVP.
- Users must only be able to access data belonging to their restaurant.

## 4. Invitations

An Invitation allows a restaurant manager to invite a new employee to join the restaurant.

### Invitation properties

- id
- restaurant_id
- email
- role
- token
- status
- expires_at
- created_at

### Status

- pending
- accepted
- expired
- cancelled

### Rules

- Only managers can create employee invitations.
- An invitation belongs to one restaurant.
- An invitation can only be accepted once.
- Expired invitations cannot be accepted.
- The invitation determines which restaurant the new employee joins.
- The invitation determines the employee's initial role.

## 5. Restaurant Areas

An Area represents a physical area of a restaurant.

Examples:

- Main Dining
- Terrace
- Bar
- Kitchen
- Entrance
- Service Area

### Area properties

- id
- restaurant_id
- name
- type
- created_at

### Rules

- An area belongs to one restaurant.
- An area cannot be accessed by another restaurant.

---

## 6. Tables

A Table represents a physical customer table inside a restaurant.

### Table properties

- id
- restaurant_id
- area_id
- number
- capacity
- position_x
- position_y
- created_at

### Rules

- A table belongs to one restaurant.
- A table belongs to one area.
- Table numbers must be unique within a restaurant.
- Position data is used to display tables on the interactive restaurant map.

## 7. Menu Categories

A Menu Category groups menu items within a restaurant.

Examples:

- Starters
- Main Courses
- Desserts
- Drinks
- Cocktails
- Coffee

### Menu Category properties

- id
- restaurant_id
- name
- display_order
- created_at

### Rules

- A category belongs to one restaurant.
- A category cannot be accessed by another restaurant.
- A category can contain multiple menu items.
- A menu item belongs to one category in the MVP.
- Categories can be reordered by the manager.

## 8. Menu Items

A Menu Item represents a food or beverage offered by a restaurant.

### Menu Item properties

- id
- restaurant_id
- category_id
- name
- description
- price
- image
- availability_status
- service_notes
- created_at
- updated_at

### Availability Status

- available
- unavailable

### Rules

- A menu item belongs to one restaurant.
- A menu item belongs to one category in the MVP.
- A manager can create, edit, or remove menu items.
- Employees can view menu items belonging to their restaurant.
- Menu information is restaurant-specific.

## 9. Ingredients

An Ingredient represents a component used in one or more menu items.

### Ingredient properties

- id
- restaurant_id
- name
- description
- created_at

### Rules

- An ingredient belongs to one restaurant.
- An ingredient may be used by multiple menu items.
- Ingredient information must be provided or verified by the restaurant.
- Employees can view ingredient information relevant to their restaurant's menu.

## 10. Menu Item Ingredients

Menu Item Ingredients connects menu items with their ingredients.

### Properties

- id
- menu_item_id
- ingredient_id
- quantity_information
- preparation_notes

### Rules

- A menu item can contain multiple ingredients.
- An ingredient can belong to multiple menu items.
- The same ingredient should not be linked to the same menu item more than once.
- Ingredient relationships belong to the same restaurant.

## 11. Allergens

An Allergen represents an allergen relevant to restaurant menu information.

### Allergen properties

- id
- name
- code
- description

### Rules

- Allergen information must be verified by the restaurant.
- ServeReady must not invent or automatically infer allergen information.
- Employees should be able to view verified allergen information.
- Employees should be instructed to confirm uncertain allergen information with authorized restaurant staff.

## 12. Ingredient Allergens

Ingredient Allergens connects ingredients with verified allergens.

### Properties

- id
- ingredient_id
- allergen_id

### Rules

- An ingredient can have multiple allergens.
- An allergen can apply to multiple ingredients.
- The same allergen should not be linked to the same ingredient more than once.
- Allergen relationships must be verified by the restaurant.

## 13. Training Modules

A Training Module represents a restaurant-specific training topic that an employee must complete.

Examples:

- Restaurant Orientation
- Table Knowledge
- Menu Knowledge
- Ingredients and Allergens
- Service Basics
- Guest Interaction

### Training Module properties

- id
- restaurant_id
- title
- description
- display_order
- is_active
- created_at
- updated_at

### Rules

- A training module belongs to exactly one restaurant.
- Training modules cannot be accessed by users from another restaurant.
- Managers can create, edit, activate, and deactivate training modules.
- Employees can access modules assigned to them.
- Training modules are completed in the order configured by the restaurant.

## 14. Lessons

A Lesson represents a learning unit inside a Training Module.

### Lesson properties

- id
- training_module_id
- title
- content
- display_order
- created_at
- updated_at

### Rules

- A lesson belongs to one training module.
- A training module can contain multiple lessons.
- Lessons have an explicit display order.
- Employees must complete required lessons before attempting the associated assessment.
- Managers can create and edit lessons belonging to their restaurant.

## 15. Questions

A Question represents an assessment question created by the restaurant.

### Question properties

- id
- training_module_id
- question_type
- question_text
- explanation
- display_order
- is_active
- created_at
- updated_at

### Question Types

The MVP supports:

- multiple_choice
- true_false
- image_identification
- table_identification
- scenario

### Rules

- A question belongs to one training module.
- Managers can create questions for their restaurant's training modules.
- Questions must belong to the same restaurant as their training module.
- Managers can deactivate questions without permanently deleting historical assessment data.

## 16. Question Options

A Question Option represents a possible answer to a question.

### Question Option properties

- id
- question_id
- option_text
- is_correct
- display_order

### Rules

- A question can have multiple options.
- Each option belongs to one question.
- Multiple-choice questions must have at least one correct option.
- The correct answer must be determined by the restaurant's configured question data.
- Employees should not receive correctness feedback until the assessment has been submitted.

## 17. Assessment Attempts

An Assessment Attempt represents one complete attempt by an employee to complete a training module assessment.

### Assessment Attempt properties

- id
- employee_id
- training_module_id
- score
- passed
- started_at
- submitted_at

### Rules

- An assessment attempt belongs to one employee.
- An assessment attempt belongs to one training module.
- Every submitted attempt is preserved.
- Employees can have multiple attempts.
- A failed attempt triggers a cooldown period before another attempt can begin.
- The initial cooldown period is 30 minutes.
- The system calculates the result automatically.

## 18. Assessment Answers

An Assessment Answer represents an employee's answer to an individual question during an assessment attempt.

### Assessment Answer properties

- id
- assessment_attempt_id
- question_id
- selected_option_id
- is_correct
- answered_at

### Rules

- An assessment answer belongs to one assessment attempt.
- An assessment answer references one question.
- The selected option must belong to the referenced question.
- The system records whether the answer was correct.
- Historical answers must not change when a manager later edits a question.

## 19. Training Progress

Training Progress represents an employee's progress through a training module.

### Training Progress properties

- id
- employee_id
- training_module_id
- status
- progress_percent
- started_at
- completed_at
- updated_at

### Status

- not_started
- in_progress
- completed

### Rules

- Each employee has one progress record per assigned training module.
- Progress is updated as the employee completes required lessons and assessments.
- A module is completed only when all required learning requirements have been satisfied.
- Completing an assessment successfully can mark the module as completed.
- Managers can view employee progress.
- Employees can view their own progress.

## 20. Training Assignments

A Training Assignment connects an employee with a training module that they are required to complete.

### Training Assignment properties

- id
- employee_id
- training_module_id
- assigned_at
- due_at
- status

### Status

- assigned
- in_progress
- completed
- cancelled

### Rules

- An assignment belongs to one employee.
- An assignment references one training module.
- A manager can assign training to an employee.
- Employees can only access training assigned to them.
- Completed assignments remain available as historical records.

## 21. Floor Map

The restaurant floor map allows employees to visually learn the physical layout of the restaurant.

### MVP

Managers configure:

- Areas
- Tables
- Table numbers
- Table capacity
- Table positions

ServeReady uses this information to display an interactive restaurant map to employees.

### Employee Experience

Employees can:

- View the restaurant map.
- Identify table numbers.
- Select tables.
- View table information.
- Learn the relationship between tables and restaurant areas.
- Complete table-identification exercises.

### Manager Experience

In the MVP, managers do not have a drag-and-drop visual map editor.

Managers enter table and position information through the management interface.

### Future Enhancement

A future version may provide a visual floor-plan editor allowing managers to:

- Drag tables.
- Move tables.
- Resize tables.
- Create areas visually.
- Add restaurant features.
- Preview the employee experience.
