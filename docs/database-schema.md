# ServeReady Database Schema

## Purpose

This document defines the relational database structure required by ServeReady.

The database must support:

- Restaurant isolation
- Employee onboarding
- Restaurant configuration
- Floor maps
- Menu knowledge
- Ingredients and allergens
- Restaurant-specific training
- Assessments
- Assessment attempts
- Employee progress
- Manager monitoring

## Design Principles

1. Every restaurant's data must be isolated.
2. Relationships between entities must be explicit.
3. Foreign keys should enforce relationships.
4. Unique constraints should prevent invalid duplicate data.
5. Historical assessment data must remain stable.
6. Authentication credentials must not be stored directly by ServeReady.
7. Restaurant-specific information belongs to the restaurant that owns it.
8. The MVP should avoid unnecessary complexity.

---

# Core Identity

## Users

Represents a person using ServeReady.

Planned fields:

- id
- name
- email
- created_at
- updated_at

Notes:

Authentication will be handled by an external authentication system such as Supabase Auth.

The ServeReady database will store the application's user profile rather than user passwords.

---

## Restaurants

Represents a restaurant using ServeReady.

Planned fields:

- id
- name
- address
- logo
- contact_information
- created_at
- updated_at

---

## Memberships

Connects a user to their restaurant and defines their role.

Planned fields:

- id
- user_id
- restaurant_id
- role
- created_at

Roles:

- employee
- manager

Rules:

- A user belongs to one restaurant in the MVP.
- A user has one primary role.
- A manager manages one restaurant in the MVP.
- An employee belongs to one restaurant in the MVP.

---

## Invitations

Allows a manager to invite an employee.

Planned fields:

- id
- restaurant_id
- email
- role
- token
- status
- expires_at
- created_at

Statuses:

- pending
- accepted
- expired
- cancelled

---

# Restaurant Structure

## Areas

Represents a physical area of a restaurant.

Planned fields:

- id
- restaurant_id
- name
- type
- created_at
- updated_at

Examples:

- Main Dining
- Terrace
- Bar
- Kitchen
- Entrance
- Service Area

---

## Tables

Represents a physical customer table.

Planned fields:

- id
- restaurant_id
- area_id
- number
- capacity
- position_x
- position_y
- created_at
- updated_at

Rules:

- Table numbers must be unique within a restaurant.
- A table belongs to one restaurant.
- A table belongs to one area.
- Position information is used for the employee-facing restaurant map.

---

# Menu

## Menu Categories

Groups menu items.

Planned fields:

- id
- restaurant_id
- name
- display_order
- created_at
- updated_at

Rules:

- A category belongs to one restaurant.
- A menu item belongs to one category in the MVP.

---

## Menu Items

Represents a food or beverage offered by a restaurant.

Planned fields:

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

---

## Ingredients

Represents an ingredient used by one or more menu items.

Planned fields:

- id
- restaurant_id
- name
- description
- created_at
- updated_at

---

## Menu Item Ingredients

Junction table connecting menu items and ingredients.

Planned fields:

- id
- menu_item_id
- ingredient_id
- quantity_information
- preparation_notes

Rules:

- A menu item can contain multiple ingredients.
- An ingredient can be used in multiple menu items.
- The same ingredient cannot be linked to the same menu item more than once.

---

## Allergens

Represents a verified allergen category.

Planned fields:

- id
- name
- code
- description

---

## Ingredient Allergens

Junction table connecting ingredients and allergens.

Planned fields:

- id
- ingredient_id
- allergen_id

Rules:

- An ingredient can have multiple allergens.
- An allergen can apply to multiple ingredients.
- The same allergen cannot be linked to the same ingredient more than once.
- Allergen information must be verified by the restaurant.

---

# Training

## Training Modules

Represents a restaurant-specific training module.

Planned fields:

- id
- restaurant_id
- title
- description
- display_order
- is_active
- created_at
- updated_at

---

## Lessons

Represents a learning unit within a training module.

Planned fields:

- id
- training_module_id
- title
- content
- display_order
- created_at
- updated_at

---

## Questions

Represents an assessment question.

Planned fields:

- id
- training_module_id
- question_type
- question_text
- explanation
- display_order
- is_active
- created_at
- updated_at

Question types:

- multiple_choice
- true_false
- image_identification
- table_identification
- scenario

---

## Question Options

Represents possible answers to a question.

Planned fields:

- id
- question_id
- option_text
- is_correct
- display_order

Rules:

- An option belongs to one question.
- Multiple-choice questions can contain multiple options.
- Correctness information is hidden from employees during the assessment.

---

# Employee Training

## Training Assignments

Connects an employee to required training.

Planned fields:

- id
- employee_id
- training_module_id
- assigned_at
- due_at
- status

Statuses:

- assigned
- in_progress
- completed
- cancelled

---

## Training Progress

Tracks an employee's progress through a training module.

Planned fields:

- id
- employee_id
- training_module_id
- status
- progress_percent
- started_at
- completed_at
- updated_at

Statuses:

- not_started
- in_progress
- completed

---

# Assessments

## Assessment Attempts

Represents one attempt at completing a training assessment.

Planned fields:

- id
- employee_id
- training_module_id
- score
- passed
- started_at
- submitted_at

Rules:

- Every submitted attempt is preserved.
- Employees may have multiple attempts.
- A failed attempt triggers a cooldown.
- Initial cooldown: 30 minutes.

---

## Assessment Answers

Represents an employee's answer to a question during an assessment.

Planned fields:

- id
- assessment_attempt_id
- question_id
- selected_option_id
- is_correct
- answered_at

Rules:

- Answers belong to an assessment attempt.
- Historical assessment data must remain stable.
- Editing a question must not change historical assessment results.

---

# Relationships

## Identity

User → Membership → Restaurant

## Restaurant Structure

Restaurant → Areas → Tables

## Menu

Restaurant → Menu Categories → Menu Items

Menu Items ↔ Ingredients

Ingredients ↔ Allergens

## Training

Restaurant → Training Modules → Lessons

Restaurant → Training Modules → Questions → Question Options

Employee → Training Assignments → Training Modules

Employee → Training Progress → Training Modules

Employee → Assessment Attempts → Assessment Answers

---

# Tenant Isolation

Every restaurant-specific record must belong to a restaurant.

Users must only be able to access data belonging to their own restaurant.

The application must enforce tenant isolation through backend authorization and database-level security mechanisms where appropriate.

A user from Restaurant A must never be able to access:

- Restaurant B employees
- Restaurant B tables
- Restaurant B menu items
- Restaurant B ingredients
- Restaurant B training
- Restaurant B assessment results

---

# Historical Data

Assessment history must remain reliable even if managers later modify:

- Questions
- Question options
- Training content
- Menu information

Historical assessment results must represent what the employee actually answered at the time of the assessment.

---

# Future Extensions

Possible future features include:

- Manager visual floor-plan editor
- Multiple restaurants per organization
- Restaurant groups
- Training module templates
- Advanced analytics
- Employee certifications
- Notifications
- Scheduling
- QR-code based training access
