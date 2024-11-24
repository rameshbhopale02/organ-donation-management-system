# Organ Donation Management System

This project is a web-based application designed to facilitate the organ donation and allocation process. The system manages donor registrations, patient registrations, and organ allocations. It includes an admin panel to oversee and manage the entire process efficiently. The application is built using HTML, CSS, JavaScript for the frontend, Flask (Python) for the backend, and MongoDB for data storage. It is deployed on AWS EC2 for cloud hosting.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

The Organ Donation Management System aims to streamline the process of organ donation by providing a centralized platform for managing donor and patient information, tracking the availability of organs, and allocating organs to patients in need.

### Key Components:
- **Home Page**: Highlights the importance of organ donation and provides information on how the system works.
- **Donor Registration**: Allows individuals to register as organ donors.
- **Patient Registration**: Enables hospitals or patients to register those in need of organ transplants.
- **Admin Panel**: A secured panel for administrators to manage organ donations, allocate organs to registered patients, and view system statistics.
- **Organ Status**: Displays available organs and matches them with registered patients based on compatibility.

## Features

- **Donor and Patient Registration**: Simple forms for registering donors and patients.
- **Organ Status Dashboard**: View the current status of available organs and their allocations.
- **Admin Authentication**: Secure login for the admin panel.
- **Organ Allocation**: Admin can allocate available organs to matching patients.
- **Responsive Design**: User-friendly interface that works across different devices.
- **Cloud Deployment**: Hosted on AWS EC2 for scalability and reliability.

## Tech Stack

### Frontend:
- **HTML, CSS, JavaScript**: Used to build the user interface.

### Backend:
- **Python (Flask)**: Backend framework for handling the server-side logic and APIs.
- **MongoDB**: NoSQL database for storing donor, patient, and organ information.

### Deployment:
- **AWS EC2**: Cloud server for hosting the application.

## Installation

### Prerequisites
Make sure you have the following installed on your machine:
- Python 3.x
- MongoDB
- Flask
- Git

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/organ-donation-system.git
   cd organ-donation-system
2. **Setup Virtualenvironment**:
    ```bash
    pip install virtualenv
    virtualenv venv
    venv\Scripts\Activate
3.  **Install the Required Libraries**:
   ```bash
   pip install -r requirements.txt
4.  **Run the Application**:
   ```bash
    python app.py

   
