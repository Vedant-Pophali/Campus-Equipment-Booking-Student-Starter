# Campus Equipment Booking Service
Java 17 / Spring Boot REST API for a CI/CD lab assessment.

## Defect Fix & Set E Change Request
- **Defect Fixed**: Corrected an off-by-one boundary condition in `BookingService.java` where requests exactly one unit greater than available inventory were incorrectly accepted.
- **Change Request (Set E)**: Implemented validation to allow only positive booking quantities. Rejected zero and negative quantities with an error response, ensuring inventory remains unaltered.
- **Testing**: Added automated tests verifying exact inventory requests, exceeding inventory, and positive/zero/negative quantities.

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
![alt text](<Screenshot 2026-10-27 093336.png>)

## Task 2,3,4 (Maven Build and Boundary Testing Evidence)
![alt text](<Screenshot 2026-10-27 093401.png>)

## Task 5(Docker Image Verification)
![alt text](<Screenshot 2026-10-27 093432.png>)

## Task 6(Kubernetes Scaling and Orchestration)
![alt text](<Screenshot 2026-10-27 093951.png>)
![alt text](<Screenshot 2026-10-27 093510.png>)
![alt text](<Screenshot 2026-10-27 093536.png>)

## Extra Task (Jenkins Evidence)
![alt text](<Screenshot 2026-10-27 100612.png>)
![alt text](<Screenshot 2026-10-27 100658.png>)
![alt text](<Screenshot 2026-10-27 100728.png>)
![alt text](<Screenshot 2026-10-27 100751.png>)
![alt text](<Screenshot 2026-10-27 100815.png>)