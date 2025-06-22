# Notifications Component - High-Level Documentation

## Overview

This component implements a notification dialog for friend requests in a React + Redux web application using Material-UI (MUI) components. It interacts with the app's Redux store and backend, allowing users to view, accept, or reject incoming friend requests.

---

## Key Responsibilities

- **Fetch Notifications:** Retrieves a list of pending friend requests from the backend.
- **Display Notifications:** Shows the requests in a material "Dialog" that pops up when triggered.
- **Accept/Reject Requests:** Provides buttons for users to accept or reject friend requests, updating the backend and closing the dialog accordingly.
- **Loading/Error Handling:** Shows a loading indicator while fetching data and processes errors with a custom error hook.
- **Redux Integration:** Reads notification dialog state from the Redux store and can update it to open/close the dialog.

---

## Main Components

### 1. `<Notifications />`

- **Dialog Trigger:** Opens or closes based on the `isNotification` value from the Redux state.
- **Data Fetching:** Uses a data-fetching hook to load notifications (friend requests).
- **Loading State:** Displays a skeleton loader while fetching.
- **Notifications List:** Displays all pending friend requests or a "0 notifications" message if none exist.
- **Accept/Reject Logic:** Defines a handler to process friend request actions; upon interaction, sends an accept/reject operation to the backend and closes the dialog.

### 2. `<NotificationItem />`

- **Displays Each Request:** Shows the sender's avatar and name, indicating a friend request was sent.
- **Action Buttons:** Renders 'Accept' and 'Reject' buttons, both of which invoke the handler passed from the parent component with appropriate parameters.
- **Memoized:** Component is memoized with `React.memo` for performance.

---

## Redux & Hooks

- **Redux State:** Uses `useSelector` to check if the notification dialog should be open, and `useDispatch` to change this state.
- **API Integration:** Relies on RTK Query hooks for data fetching and mutations.
- **Custom Hooks:** Uses hooks for async mutation handling (`useAsyncMutation`) and error processing (`useErrors`).

---

## Key Technologies

- **React**
- **Redux Toolkit (RTK Query)**
- **Material-UI**
- **Custom hooks for async and error handling**

---

## User Flow

1. **Notification Available:** If a user receives a friend request, a notification popup dialog can be shown.
2. **Viewing Notifications:** The dialog lists all pending friend requests.
3. **Handling Requests:** The user can accept or reject each request directly from the dialog.
4. **State Management:** Actions update backend data and control the visibility of the dialog.

---

## Customization Notes

- The styling, dialog behavior, and logic are based on Material-UI and Redux approaches but can be easily adapted to suit different app requirements.
- Error handling and async mutation flow are abstracted into custom hooks for reuse.