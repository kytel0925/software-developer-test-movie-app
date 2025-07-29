## Software Developer Design Test "Cinema Booking System"

### Description

Design a booking system for a major cinema chain in Santander/Spain. The cinema has multiple projection rooms, each with several movie screenings throughout the day. Customers should be able to search for and book seats for a specific movie screening.

### Functional Requirements

#### Movie and Screening Management:

* The system must allow for adding, updating, and deleting movies.
* The system must allow for scheduling screenings for movies in different rooms and at different times.

#### Search and Bookings:

* Customers must be able to search for movie screenings by title and date.
* Customers must be able to view screening details, including available seats.
* Customers must be able to book specific seats for a movie screening.
* The system must allow for booking cancellations.

#### Authentication and User Management:

* Users must be able to register and log in.
* Users must be able to view and manage their bookings.

### Additional Context for Potential Traffic

Based on studies and data from the cinema industry, on average, a cinema can receive between 300 and 2,000 people per day on a Friday. Here's a more specific breakdown:

#### Cinema in a Large City/Metropolitan Area (Santander/Spain)

* **Average Daily Attendance:** 2,000 - 5,000 people or more, especially with major releases.

It's expected that 80% of these cinema-goers will purchase tickets online.

#### Calculation Example

For a cinema with 10 screens in a medium-sized city, where each screen has 150 seats and 5 screenings per screen on a Friday:

* **Total Screenings:** 10 screens * 5 screenings = 50 screenings
* **Total Capacity:** 50 screenings * 150 seats = 7,500 seats
* If the cinema has an average occupancy rate of 60%:
    * **Estimated Attendance:** 7,500 seats * 60% = 4,500 people

### Deliverables

* **Entity Relationship Diagram:** Of all related table to this solution.
* **Classes Design:** Classes with method descriptions.
* **Sequences Diagram:** For two use cases "reservation of a movie screening" and "cancel of a reservation"
* **State Diagram:** For all the main objects in the previous use cases

You MUST set up your deliverables using [mermaid](https://mermaid.js.org/intro/) in this README file.

**Please Note:**

This test does not require a technical implementation at the code level. The solution should only be presented with the requested deliverables.

We'll discuss the solution design further during the interview. We can also work together during the interview to improve the proposed solution.

## Final concerns

When delivering the solution, please keep in mind the following practices:

* You MUST create a new branch named `feature/{first-name}-{last-name}-task-list`.
* You MUST assign a pull request from your new branch to the original project.
* The solution must be written in English.
