# Campus Equipment Booking Service
Java 17 / Spring Boot REST API for a CI/CD lab assessment.

## Defect Analysis & Boundary Testing (Set E)

**1. The Initial State**
The `BookingService.java` handled equipment reservations by comparing the requested quantity against the currently available in-memory inventory, but the core validation logic was flawed and incomplete.

**2. The Error & Failing Tests**
Running boundary test cases against the initial code resulted in failures due to two specific logic gaps:
* **Off-by-One Defect:** The inventory check condition allowed a booking to proceed if the requested amount was exactly one unit greater than the available stock. A test requesting 6 items when only 5 were available failed because the application incorrectly accepted the booking instead of rejecting it.
* **Missing Positive Validation (Set E):** The system did not inspect the quantity for valid real-world numbers. Tests requesting `0` or `-2` items failed because the service processed them instead of blocking them, which would corrupt the inventory count (e.g., adding to the stock by subtracting a negative number).

**3. The Changes Made**
* **Strict Boundary Enforcement:** Corrected the conditional logic to strictly evaluate `if (requestQuantity > availableInventory)` before allowing the booking to proceed.
* **Input Sanitization:** Added a guard clause `if (requestQuantity <= 0)` at the start of the service method to immediately reject zero or negative requests with a validation error.
* **Test Coverage:** Added specific JUnit test methods to verify exact capacity requests, off-by-one excess requests, and non-positive requests.

**4. The Result**
The Spring Boot application now securely protects the inventory state from invalid data. Running the automated test suite (`mvn clean test`) results in all 9 tests passing successfully, providing concrete proof that the service correctly accepts valid bookings while throwing the appropriate exceptions for out-of-bounds parameters.

## Run Natively
`mvn spring-boot:run` — port 8080.

## Build & Test
```bash
mvn clean test
mvn clean package
```

## Docker Build & Run
```bash
    docker build -t campus-equipment-booking:set-e .
    docker run -d -p 8081:8080 --name campus-equipment campus-equipment-booking:set-e
```

## Kubernetes Deployment (Minikube)
```bash
    minikube image load campus-equipment-booking:set-e
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    kubectl scale deployment campus-equipment-deployment --replicas=2
```

## Health Check Access
```bash
    kubectl port-forward service/campus-equipment-service 8081:8080
    # Access via: http://localhost:8081/actuator/health
```

## Endpoints
- GET /api/equipment
- GET /api/equipment/{id}
- POST /api/bookings
- GET /api/bookings/{id}
- POST /api/bookings/{id}/cancel
- GET /actuator/health
Inventory is in memory and resets on restart. No database is required.

## Task 1(Git Branching and History Evidence)
![alt text](<pics/Screenshot 2026-10-27 093336.png>)

## Task 2,3,4 (Maven Build and Boundary Testing Evidence)
![alt text](<pics/Screenshot 2026-10-27 093401.png>)

## Task 5(Docker Image Verification)
![alt text](<pics/Screenshot 2026-10-27 093432.png>)

## Task 6(Kubernetes Scaling and Orchestration)
![alt text](<pics/Screenshot 2026-10-27 093951.png>)
![alt text](<pics/Screenshot 2026-10-27 093510.png>)
![alt text](<pics/Screenshot 2026-10-27 093536.png>)

## Extra Task (Jenkins Evidence)
![alt text](<pics/Screenshot 2026-10-27 100612.png>)
![alt text](<pics/Screenshot 2026-10-27 100658.png>)
![alt text](<pics/Screenshot 2026-10-27 100728.png>)
![alt text](<pics/Screenshot 2026-10-27 100751.png>)
![alt text](<pics/Screenshot 2026-10-27 100815.png>)