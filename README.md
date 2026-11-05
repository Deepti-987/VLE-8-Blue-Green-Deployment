# VLE-8: Blue-Green Deployment using DevOps Tools

## Objective
To implement Blue-Green Deployment using Docker, Kubernetes,
Jenkins, GitHub, Terraform and Ansible.

## Project Description
This project maintains two application environments:

- Blue: Current live application (Version 1)
- Green: New application (Version 2)

Kubernetes Service switches traffic between Blue and Green.
Rollback is possible by switching traffic back to Blue.

## Technologies Used
- AWS EC2
- Ubuntu
- Docker
- Kubernetes (Minikube)
- Jenkins
- GitHub
- Terraform
- Ansible

## Application Versions
- Blue: myapp:v1
- Green: myapp:v2

## Deployment
Kubernetes runs two replicas of each application.
The Service initially routes traffic to Blue.

## Traffic Switching
Switch to Green:
kubectl patch service myapp-service -p '{"spec":{"selector":{"app":"myapp","color":"green"}}}'

Rollback to Blue:
kubectl patch service myapp-service -p '{"spec":{"selector":{"app":"myapp","color":"blue"}}}'
