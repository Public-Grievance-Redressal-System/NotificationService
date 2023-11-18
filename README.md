# NotificationService
Notification Service Repository
## Description
This microservice is designed to handle notifications for SMS channels. It provides functionality to send notifications and fetch notification history for a user.

## Table of Contents
- Prerequisites
- Installation
- Usage
- Configuration
- Endpoints
- DTO Classes
- Exception Handling
- Model Classes
- Repositories
- Services
## Prerequisites
Before you begin, ensure you have met the following requirements:

- Java 17 or later
- Maven
- Twilio Account (for SMS notifications)
## Usage
This microservice code can be integrated with any other service or application. By integrating this microservice and providing apprpriate Twilio account credentials, it can be used to send SMS notifications by calling the provided APIs.

## Installation
1. Clone the repository:

```git clone https://github.com/Public-Grievance-Redressal-System/NotificationService.git```

2. Build and rund the project.

## Configuration
Ensure to configure the following properties in your application.properties or application.yml:

- twilio.account.sid: Twilio Account SID
- twilio.auth.token: Twilio Auth Token

These can be stored as environmental variables and then retrived as required to initialize the integrated twilio service.

## Endpoints
- Send Notification:

`POST /notifications/send`

Request Body:

```
{
    "requestingServiceId": 1,
    "requestingServiceNotificationId": 123,
    "userId": 456,
    "messageTitle": "Notification Title",
    "messageContent": "Notification Content",
    "notificationChannel": "SMS"
}
```
## Fetch Notification History:
`POST /notifications/fetch/user/{id}`

Example:
`POST /notifications/fetch/user/456`

## DTO Classes
### Send Notification Request Dto
```
@Getter
@Setter
public class SendNotificationRequestDto {
    // Properties...
}
```
### MessageDto
```
@Getter
@Setter
public class MessageDto {
    // Properties...
}
```
### ExceptionDto
```
public class ExceptionDto {
    // Properties...
}
```
### Exception Handling
Handle exceptions with 'ControllerAdvices':
```
@ControllerAdvice
public class ControllerAdvices {
    // Exception handling methods...
}
```
## Model Classes

### Base Model

```
@MappedSuperclass
@Getter
@Setter
public abstract class BaseModel {
    // Properties...
}
```
### Notification

```
@Getter
@Setter
@Entity
@AllArgsConstructor
@NoArgsConstructor
public class Notification extends BaseModel implements Comparable<Notification> {
    // Properties...
}
```
### Message
```
@Entity
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class MockUser extends BaseModel {
    // Properties...
}
```

## Repositories

### MockUserRepository

```
public interface MockUserRepository extends JpaRepository<MockUser, Long> {
    // Repository methods...
}
```
### Notification
```
@Repository
public interface NotificationRepository extends JpaRepository<Notification, Long> {
    // Repository methods...
}
```
## Services

### MockUserService

### NotificationService

## Twilio API Integration

```
@RestController
public class SmsController {
    @GetMapping(value = "/sendSMS")
    public ResponseEntity<String> sendSMS(String fromNumber, String toNumber, String message) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter twilio account sid: ");
        String twilioAccountSid = sc.nextLine();
        System.out.println("Enter twilio auth token: ");
        String twilioAuthToken = sc.nextLine();
        Twilio.init(twilioAccountSid, twilioAuthToken);
        Message.creator(new PhoneNumber(toNumber),
                new PhoneNumber(fromNumber), message).create();
        return new ResponseEntity<>("Message sent successfully to number: " + toNumber, HttpStatus.OK);
    }
}
```
