# Project_git_action_AWS

**An automated CI/CD Flask REST API deployment pipeline to AWS ECS using GitHub Actions**

## Overview

This project demonstrates a production-ready deployment workflow for a Flask-based REST API to Amazon Web Services (AWS) using GitHub Actions. It features:

- ✅ **REST API** for student management built with Flask
- ✅ **PostgreSQL Integration** with AWS RDS for persistent data storage
- ✅ **Docker Containerization** for consistent environments
- ✅ **Automated CI/CD Pipeline** using GitHub Actions
- ✅ **AWS Deployment** to ECS cluster with automatic image updates

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Language** | Python 3.11 (92.1%) |
| **Framework** | Flask 3.0.3 |
| **Database** | PostgreSQL (AWS RDS) |
| **Container** | Docker |
| **Container Registry** | AWS ECR Public |
| **Orchestration** | AWS ECS |
| **CI/CD** | GitHub Actions |
| **Cloud Region** | ap-south-2 (Mumbai) |

## Project Structure

```
Project_git_action_AWS/
├── .github/
│   └── workflows/
│       └── deploy.yml              # Automated deployment workflow
├── app.py                          # Flask API application
├── Dockerfile                      # Docker image configuration
├── requirements.txt                # Python dependencies
├── Output_images/                  # Documentation & screenshots
└── README.md                       # This file
```

## API Endpoints

### Health Check
```http
GET /hello
```
**Response:**
```json
{
  "sharlin": "Hello benanio"
}
```

### Get All Students
```http
GET /students
```
**Response:**
```json
[
  {"id": 1, "name": "John Doe", "age": 20},
  {"id": 2, "name": "Jane Smith", "age": 21}
]
```

### Add a Student
```http
POST /students
Content-Type: application/json

{
  "name": "Alice Johnson",
  "age": 22
}
```
**Response:**
```json
{
  "message": "Student added successfully!",
  "student_id": 3
}
```

## Getting Started

### Prerequisites
- Python 3.11+
- Docker (for containerization)
- AWS Account with ECS cluster configured
- GitHub repository with Actions enabled

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/Benaniosam-hub/Project_git_action_AWS.git
   cd Project_git_action_AWS
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure environment variables**
   ```bash
   export DB_HOST=your-rds-endpoint.rds.amazonaws.com
   export DB_NAME=postgres
   export DB_USER=postgres
   export DB_PASSWORD=your-password
   ```

4. **Run the application**
   ```bash
   python app.py
   ```
   The API will be available at `http://localhost:5000`

### Testing Locally

```bash
# Test health endpoint
curl http://localhost:5000/hello

# Retrieve all students
curl http://localhost:5000/students

# Add a new student
curl -X POST http://localhost:5000/students \
  -H "Content-Type: application/json" \
  -d '{"name": "Test Student", "age": 20}'
```

## Deployment

### Docker Build & Run

```bash
# Build the Docker image
docker build -t flask-api:latest .

# Run the container locally
docker run -p 5000:5000 \
  -e DB_HOST=your-rds-endpoint \
  -e DB_NAME=postgres \
  -e DB_USER=postgres \
  -e DB_PASSWORD=your-password \
  flask-api:latest
```

### Automated AWS Deployment (GitHub Actions)

The deployment pipeline is fully automated via GitHub Actions:

1. **Trigger:** Push to the `main` branch
2. **Steps:**
   - Checkout code
   - Configure AWS credentials
   - Build Docker image
   - Push to AWS ECR Public
   - Download current ECS task definition
   - Update task definition with new image
   - Deploy to ECS cluster

#### Setup Instructions

1. **Create GitHub Secrets** in your repository:
   - `AWS_ACCESS_KEY_ID` — Your AWS IAM access key
   - `AWS_SECRET_ACCESS_KEY` — Your AWS IAM secret key

2. **Configure AWS Resources:**
   - ECS Cluster: `flask-api-cluster`
   - ECS Service: `flask-api-task-service`
   - Task Definition: `flask-api-task`
   - ECR Repository: `flask-api` (in AWS ECR Public)

3. **Environment Configuration:**
   - AWS Region: `ap-south-2` (Mumbai)
   - Container Name: `flask-api-container`
   - ECR Registry Alias: `o2e0w5z3`

4. **Push to deploy:**
   ```bash
   git add .
   git commit -m "Update deployment"
   git push origin main
   ```

   The workflow will automatically run and deploy your changes to AWS ECS.

## How the Deployment Works

```
┌─────────────────────┐
│   Git Push (main)   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────────────────┐
│   GitHub Actions Workflow       │
├─────────────────────────────────┤
│ 1. Build Docker Image           │
│ 2. Push to ECR Public           │
│ 3. Update Task Definition       │
│ 4. Deploy to ECS Cluster        │
└──────────┬──────────────────────┘
           │
           ▼
┌─────────────────────────────────┐
│   AWS ECS Cluster               │
│   (ap-south-2 / Mumbai)         │
│   - Flask API running           │
│   - Connected to RDS Database   │
└─────────────────────────────────┘
```

## Environment Variables

| Variable | Default | Description |
|----------|---------|------------|
| `DB_HOST` | `database-1.c7wqac8my5ba.ap-south-2.rds.amazonaws.com` | PostgreSQL RDS endpoint |
| `DB_NAME` | `postgres` | Database name |
| `DB_USER` | `postgres` | Database user |
| `DB_PASSWORD` | `benaniosam` | Database password (change in production) |

## Security Considerations

⚠️ **Important:** The default credentials in `app.py` are hardcoded for demo purposes. For production:

1. **Move secrets to environment variables** or use AWS Secrets Manager
2. **Use strong database passwords**
3. **Enable VPC security groups** for RDS
4. **Restrict IAM permissions** to the minimum required
5. **Enable HTTPS/TLS** for API communications
6. **Rotate AWS credentials** regularly

## Troubleshooting

### Database Connection Errors
- Verify RDS endpoint and port accessibility
- Check security group rules allow traffic from ECS cluster
- Confirm database credentials

### ECS Deployment Failures
- Check CloudWatch logs for detailed error messages
- Verify ECR image is accessible
- Confirm IAM role permissions

### GitHub Actions Workflow Failures
- Review workflow logs in Actions tab
- Verify AWS credentials are correctly set as secrets
- Check AWS region consistency

## Future Enhancements

- [ ] Add authentication/authorization endpoints
- [ ] Implement input validation and error handling improvements
- [ ] Add database migrations with Alembic
- [ ] Create comprehensive logging and monitoring
- [ ] Add unit and integration tests
- [ ] Implement request rate limiting
- [ ] Add API documentation with Swagger/OpenAPI
- [ ] Setup health checks and auto-scaling policies

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Push and create a Pull Request

## License

This project is open source. Feel free to use it for learning and development.

## Author

**Benaniosam** — [GitHub Profile](https://github.com/Benaniosam-hub)

## Support

For issues or questions, please open an issue on the [GitHub repository](https://github.com/Benaniosam-hub/Project_git_action_AWS/issues).

---

**Last Updated:** June 2026  
**Version:** 1.0.0
