# Object-Oriented Design 

### Idea at the Highest Level 

1. Determine the classes you need, what data they hold, and how they interact with one another 

2. Determine each class's data structures and algorithms 

### Class Design Steps 

1. Determine the classes and objects required 

2. Determine outward-facing functionality (public interface) 

    How does a user interact with a class? 

3. Determine the data each class holds 

4. How data interact with each other 

---

## Example: Electronic Calendar 

### Description 

Each user's clanedar should contain appointements for that user. 

There are 2 different types of appointments: one-time appts and recurring appts 

Users of the calendar can get a list of appointments for the day, add new appointments, remove existing appointments, and check other users' calendar to see if a time-slot is empty. 

The user of the calendar must supply a password before accessing the calendar.

Each appointment has a start-time and an end-time, a list of participants, and a location. 

### Step 1: Identify Objects 

First identify all the nouns in the specification: 

```
user
calendar 
appointments 
one-time appts 
recurring appts 
time-slot
password 
start-time 
end-time 
participants 
location 
```

- Calendar 

- Appointments 

- Recurring Appointment 

- One-time Appointment 

### Step 2: Identify Operations 

First identify all the verb phrases: 
```
get a list of appointments
add new appointments 
remove exisitng appointments 
check other users' calendars 
supply a password 
heach appointment has a start-time and an end-time 
a list of participants and a location 
```

Determine what actions go with which classes 
```
Calendar
{
    list getListOfAppts(void)
    bool addAppt(Appointment *addme)
    bool removeAppt(string& apptName)
    bool checkCalendars(Time& slot, Calendar others[])
    bool login(string &pass)
    bool logout(void)

    //Housekeeping 
    Calendar() 
    ~Calendar()
}

Appointment
{
    bool setStartTime(Time& st)
    bool setEndTime(Time& st)
    bool addParticipant(string& user)
    bool setLocation(string& location)

    //Housekeeping 
    Appointment()
    ~APpointment()
}
```

Two more classes: `one-time appointment` is not needed because same as `Appointment`, `recurring-appointment` 

### Step 3: Determine Relationships & Data 

1. Uses: Class X **uses** objects of class Y, but may not actually hold objects of class Y 

2. Has-A: Class X **contains** one or more instances of class Y 

3. Is-A: Class X is a **specialized verision** of class Y 

Calendar 

- Contains `appointments` 

- Has a `password` 

- Uses other `calendars`, but does not need to hold them 

```
private: 
    Appointment m_app[100]; 
    String m_password; 
```

Appointment 

- Contains `start time` and `end time` 

- Associated with a set of `participants` 

- Held at a `location` 

```
private: 
    Time m_startTime; 
    Time m_endTime; 
    string m_participants[10]; 
    string m_location; 
```

Recurring Appointment 

- Shares all attributes of an `Appointment` -> inherit! 

```
RecurringAppointment : public Appointment 
{
    ...
    private:
        int m_numDays; 
}
```
> Remember to have virtual destructors for inheritance 

### Step 4: Determine Interactions 

Come up with use cases 

1. User wants to *add an appointment* to their calendar 

2. The user wants to *determine if they have an appointment at 5pm with Joe* 

3. The user wants to *locate the appointment at 5pm* and *update it to 6pm* 

Use Case #1 User wants to add an appointment 

1. User creates a new `Appointment` object and sets its values 

    ```
    Appointment* app = new Appointment("10am","11am", "Dodd",...);  
    ```

2. User adds `Appointment` object to the `Calendar` 

    ```
    Calendar c; 
    c.addAppointment(app); 
    ```

Use Case #2 User wants to determine if they have an appointment at 5pm with Joe 

- To check time, create new function 

    ```
    Appointment* checkTime(Time& t); in class Calendar
    ```
    ```
    Calendar c; 
    Appointment* appt; 
    appt = c.checkTime("5pm"); 
    if (appt == NULL)
        cout << "No appt at 5pm"; 
    ```
- To check person, create new function in `Appointment`
    ```
    bool isAttendee(string& person); 
    ```
    ```
    else if (appt->isAttendee("Joe"))
        cout << "Joe is attending!"; 
    ```
---
## Class Design Tips 

### Tip 1: Avoid using dynamic cast to identify common types of objects. Instead add methods to check for verious classes of behaviors 

