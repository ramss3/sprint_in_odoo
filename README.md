**Odoo Sprint Management Module**

This module introduces a fully-featured Sprint system into Odoo Projects, designed based on Agile Scrum practices while enforcing strong data integrity at the ORM level.

It enables teams to plan, manage, and evolve tasks within projects using an agile workflow, while ensuring consistency, traceability, and have a better understanding of the working progress for future analysis.

🔑 ***Key Features***

🗓 ***Sprint Lifecycle Management***

- Default sprint duration of 2 weeks
- Sprint duration can be manually adjusted up to 4 weeks
- Automation sprint state calculation based on sprint dates
  - Planned
  - Active
  - Done
- Sprints cannot overlap within the same project
- Daily cron job keeps sprint states synchronized with the current date

 🔒 ***Sprint Integrity Rules***

 - A sprint can only belong to one project
 - Only one active sprint per project
 - Past sprints cannot be manually set to Planned or Active
 - Tasks can only be assigned to a sprint after the sprint’s project is defined
 - Once a sprint has tasks, its project cannot be changed
 - A sprint’s project cannot be changed once the sprint is Active or Done
 - Sprint end date behavior:
   - Automatically calculated from start date (+14 days) by default
   - User may manually override the end date
   - If manually overridden, the end date will no longer auto-update
   - Auto-updates resume if the end date is set back to the default value
 - Auto-updates resume if the end date is set back to the default value

📋 ***Task ↔ Sprint Integration***

**Automatic Deadline Management**

- When a task is assigned to a sprint:
  - Its deadline is automatically set to match the sprint end date
  - Deadline is marked as auto-managed
  - Deadline can be manually changed as long as it's within the sprint bounds
- When a sprint's end date changes:
  - All non-manual task deadlines are automatically synchronized once the sprint is saved

**Manual Task Deadline**

  - Users may manually changes task deadlines within a sprint at their willing
  - Manual deadlines are respected as long as they stay within sprint bounds
  - Sprint date changes are blocked if they would invalidate manual task deadlines

**Task's Sprint Switching Logic

  - Tasks can only be moved across sprints within the same project
  - When a task is moved to a another sprint:
    - Any previous manual deadline is discarded as sprint's within the same project cannot overlap
    - Deadline is set to the new sprint's end date
    - Deadline is marked as auto-managed

📘 ***Usage Guide***

This section explains how to correctly use the Sprint functionality and what behavior to expect.

⚙️ **Initial Setup / First-Time Use**

To correctly use this module after installation:
  1. Install the module inside an Odoo database (Make sure Project is enabled)
  2. Create a project
  3. Create one or more sprints for that project
  4. Assign tasks to sprints using the Sprint or Task form
  5. Manually update sprint states update if needed

🔄 **Creating a Sprint**
  1. Go to Projects -> Sprints
  2. Click Create
  3. Set:
     - Sprint name
     - Project
     - Start date
  4. Sprint state is automatically set based on the defined sprint window
  5. The end date is automatically set to 14 days after the start date (sprint default length)
     - Adjust the end date (up to 4 weeks)
     - Switch sprint state mode to Manual if needed

  📌 **Notes:**
    - Sprints cannot overlap within the same project
    - A sprint's project cannot be changed once tasks are assigned or sprint state is Active/Done

📎 **Attaching Tasks to a Sprint**

There are two supported ways:
  - Sprint Form:
    - Open a sprint
    - Use the Tasks tab to attach existing project tasks
    - Create new tasks that will be automatically assigned to the defined sprint's project

   - Task Form:
     - Open a task
     - Select a sprint belonging to the same project

  📌 **Rules:**
    - Tasks must belong to the same project as the sprint
    - Task project becomes read-only when editing from inside a sprint

🔄 **Moving Tasks Between Sprints**
  - Tasks can only be moved between sprints within the same project
  - When a task is moved:
    - Any previous manual deadline is discarded
    - Deadline is reset to the new sprint’s end date
    - Deadline is marked as auto-managed

***🧠 Business Logic at ORM Level***

All critical rules are enforced at the ORM level, to guarantee data integrity across all entry points (UI, imports, automation):

  - Sprint project cannot be changed once tasks are assigned
  - Sprint state constraints for past dates
  - Task deadlines must fall within sprint dates
  - Task project must match sprint project
  - Sprint date changes validate existing task deadlines


🛠 ***What Can Be Extended Next***

This solution was designed to be extensible. Possible next steps include:

  - Sprint burndown charts
  - Sprint notes / retrospectives
  - WIP limits sprint support
  - Sprint-level reporting dashboards
