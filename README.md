# Software Developer Design Test "Cinema Booking System"

## Entity Relationship Diagram

```mermaid
erDiagram
    USER {
        int id PK
        string name
        string email
        string passwordHash
    }

    MOVIE {
        int id PK
        string title
        string genre
        int durationMinutes
    }

    ROOM {
        int id PK
        string name
        int totalSeats
    }

    SCREENING {
        int id PK
        int movieId FK
        int roomId FK
        datetime startTime
    }

    SEAT {
        int id PK
        int roomId FK
        string seatNumber
    }

    BOOKING {
        int id PK
        int userId FK
        int screeningId FK
        datetime bookingTime
        string status
    }

    BOOKED_SEAT {
        int id PK
        int bookingId FK
        int seatId FK
        int screeningId FK
    }

    USER ||--o{ BOOKING : makes
    MOVIE ||--o{ SCREENING : "is shown in"
    ROOM ||--o{ SCREENING : hosts
    SCREENING ||--o{ BOOKING : allows
    SCREENING ||--o{ BOOKED_SEAT : reserves
    ROOM ||--o{ SEAT : contains
    SEAT ||--o{ BOOKED_SEAT : "is reserved"
    BOOKING ||--o{ BOOKED_SEAT : includes
```

## Class diagram

```mermaid
classDiagram
    class User {
        int id
        string name
        string email
        string passwordHash
        register()
        login()
        viewBookings()
    }

    class Movie {
        int id
        string title
        string genre
        int durationMinutes
        create()
        update()
        delete()
        getDetails()
    }

    class Room {
        int id
        string name
        int totalSeats
        addSeat(seatNumber)
        getSeatMap()
    }

    class Seat {
        int id
        string seatNumber
        isAvailable(screening)
    }

    class Screening {
        int id
        int movieId
        int roomId
        datetime startTime
        listAvailableSeats()
        getDetails()
        search(title, date)
    }

    class Booking {
        int id
        int userId
        int screeningId
        datetime bookingTime
        string status
        createBooking(seatIds)
        cancelBooking()
    }

    class BookedSeat {
        int id
        int bookingId
        int seatId
        int screeningId
    }

User      "1"  o-- "0..*" Booking      : makes
Screening "1"  *-- "0..*" Booking      : allows
Booking   "1"  *-- "1..*" BookedSeat   : includes
Screening "1"  *-- "0..*" BookedSeat   : allocates
Movie     "1"  *-- "0..*" Screening    : is shown in
Room      "1"  o-- "0..*" Screening    : hosts
Room      "1"  *-- "1..*" Seat         : contains
Seat      "1"  --> "0..*" BookedSeat   : is reserved in
Screening "1"  --> "0..*" Seat         : uses
```

## Sequence diagrams
### Reservation of movie screening
```mermaid
sequenceDiagram
    actor User
    participant WebApp as Web UI
    participant ScreeningService as ScreeningService
    participant SeatService as SeatService
    participant BookingService as BookingService

    User ->> WebApp: login(email, password)
    WebApp -->> User: loginSuccess | loginFailure

    User ->> WebApp: searchScreenings(title, date)
    WebApp ->> ScreeningService: findScreenings(title, date)
    ScreeningService -->> WebApp: screenings

    User ->> WebApp: chooseScreening(screeningId)
    WebApp ->> ScreeningService: getScreening(screeningId)
    ScreeningService ->> SeatService: getAvailableSeats(screeningId)
    SeatService -->> ScreeningService: seats
    ScreeningService -->> WebApp: seats

    User ->> WebApp: selectSeats(seatIds)
    WebApp -->> User: showSelectionSummary
    User ->> WebApp: confirmBooking
    WebApp ->> BookingService: createBooking(userId, screeningId, seatIds)
    BookingService ->> SeatService: validateSeats(screeningId, seatIds)

    alt seats available
        SeatService -->> BookingService: seatsAvailable
        BookingService ->> SeatService: lockSeats(screeningId, seatIds)
        BookingService -->> WebApp: bookingSuccess(bookingId)
        WebApp -->> User: displayConfirmation(bookingId)
    else seats unavailable
        SeatService -->> BookingService: seatsUnavailable
        BookingService -->> WebApp: bookingError
        WebApp -->> User: displayError
    end
```
### Cancelation of a reservation
```mermaid
sequenceDiagram
    actor User
    participant WebApp as "Web UI"
    participant BookingService
    participant SeatService

    User ->> WebApp: login(email, password)
    WebApp -->> User: loginSuccess | loginFailure

    User ->> WebApp: viewBookings
    WebApp ->> BookingService: getBookings(userId)
    BookingService -->> WebApp: bookings
    User ->> WebApp: cancelBooking(bookingId)

    WebApp ->> BookingService: cancelBooking(bookingId)
    BookingService ->> SeatService: releaseSeats(bookingId)
    SeatService -->> BookingService: seatsReleased
    BookingService -->> WebApp: cancellationSuccess
    WebApp -->> User: displayCancellation()
```

## State diagrams
### Booking state diagram
```mermaid
stateDiagram
    [*] --> Created : user creates booking
    Created --> Confirmed : selected seats
    Created --> Cancelled : user cancels
    Created --> Expired : booking timeouts
    Confirmed --> Cancelled : user cancels
    Confirmed --> Completed : screening starts
    Cancelled --> [*]
    Expired   --> [*]
    Completed --> [*]
```
### Seat state diagram
```mermaid
stateDiagram
    [*] --> Available
    Available --> Locked : seats selected
    Locked --> Available : booking cancelled
    Locked --> Reserved : booking confirmed
    Locked --> Available : booking timeout
    Reserved --> [*]
```