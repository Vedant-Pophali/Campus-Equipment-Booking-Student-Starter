# Campus Equipment Booking Service
Java 17 / Spring Boot REST API for a CI/CD lab assessment.

## Build and test
`mvn clean test`

## Run
`mvn spring-boot:run` — port 8080.

## Endpoints
- `GET /api/equipment`
- `GET /api/equipment/{id}`
- `POST /api/bookings`
- `GET /api/bookings/{id}`
- `POST /api/bookings/{id}/cancel`
- `GET /actuator/health`

Inventory is in memory and resets on restart. No database is required.

# Campus Equipment Booking Service

## Defect Fix & Set E Change Request
* **Defect Fixed**: Corrected an off-by-one boundary condition in `BookingService.java` where requests exactly one unit greater than available inventory were incorrectly accepted.
* **Change Request (Set E)**: Implemented validation to allow only positive booking quantities. Rejected zero and negative quantities with an error response, ensuring inventory remains unaltered.
* **Testing**: Added automated tests verifying exact inventory requests, exceeding inventory, and positive/zero/negative quantities.

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