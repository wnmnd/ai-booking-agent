# AI-Powered Inbound Call Booking Assistant

This project implements an AI-powered assistant designed to handle inbound calls for appointment booking, specifically for a dialysis center. It leverages a combination of Retell AI for voice interaction, Google Calendar for availability checks, Airtable for CRM, and Microsoft Outlook for notifications. The core logic is orchestrated using n8n, a workflow automation tool.

---

## Features

* **Intelligent Call Handling**: Utilizes Retell AI to interact with callers, understand their intent, and extract key information for appointment booking.
* **Real-time Availability Check**: Integrates with Google Calendar to check for available time slots based on caller preferences.
* **Automated Appointment Booking**: Creates events in Google Calendar for confirmed appointments.
* **CRM Integration**: Records appointment details and follow-up requirements in Airtable.
* **Email Notifications**: Sends automated email notifications via Microsoft Outlook for both successfully booked appointments and cases requiring manual follow-up.
* **Conditional Logic**: Employs an "Availability Router" to direct the workflow based on calendar availability (i.e., if a slot is available, book it; otherwise, flag for follow-up).
* **Detailed Data Extraction**: Extracts comprehensive caller information such as full name, location, contact number, payment method, preferred schedule, date, and special needs.
* **Empathetic AI Agent**: The "Single-Prompt Agent" is configured with a professional, empathetic, and polite persona ("Orion") to assist patients, providing accurate information and guiding them through the booking process.
* **Multilingual Support**: The AI agent can communicate in both Malay and English, adapting to the caller's preference.

---

## Architecture Overview

The system is built around an n8n workflow that orchestrates various services:

1.  **Retell AI Webhook**: This is the entry point for inbound calls. Retell AI handles the voice interaction, transcribes the conversation, and extracts structured data (e.g., patient details, preferred appointment time).
2.  **Google Calendar**:
    * `Get availability in a calendar`: Checks the Google Calendar for the requested date and time slot.
    * `Create an event`: Books the appointment in the calendar if the slot is available.
3.  **Availability Router (n8n If Node)**: This node acts as a decision point.
    * If the requested time slot is available (checked via Google Calendar), the workflow proceeds to book the appointment.
    * If the slot is unavailable, the workflow branches to trigger a manual follow-up process.
4.  **Airtable**:
    * `Airtable - Appointment Booked`: Records details of successfully booked appointments.
    * `Airtable - Follow-up Required`: Logs cases where an appointment could not be immediately booked, requiring manual intervention.
5.  **Microsoft Outlook**:
    * `Send a message1`: Sends an email notification for successfully booked appointments.
    * `Send a message`: Sends an email notification to the relevant team when a follow-up is required.
6.  **Webhook Response**: Sends a JSON response back to Retell AI, indicating the status of the booking (success or follow-up required).

---
