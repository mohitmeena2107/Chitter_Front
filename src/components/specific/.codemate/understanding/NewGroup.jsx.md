# NewGroup Component — High-Level Documentation

## Overview
The `NewGroup` component provides a modal dialog for creating a new group chat. It enables users to specify a group name and select group members from their list of available friends. The modal handles validation, asynchronous requests, and error states to ensure a smooth user experience.

---

## Major Features

- **Modal UI**: Uses MUI's `<Dialog>` and form controls to display a styled modal for group creation.
- **Input Validation**: Validates group name input via a custom hook.
- **Friend List Fetching**: Retrieves available friends from the backend for potential group inclusion.
- **Member Selection**: Allows the user to select at least two friends (for a group of at least three people).
- **Error Management**: Handles and displays errors related to API calls and input validation.
- **Submission Handling**: Submits new group data asynchronously and provides feedback during the operation.

---

## Key Functional Flow

1. **Modal Visibility**  
   Controlled via Redux state. Dispatches an action to open or close the modal.

2. **Input for Group Name**  
   Utilizes a controlled text field and real-time validation for empty value.

3. **Fetching & Displaying Friends List**  
   - Uses an API hook (`useAvailableFriendsQuery`) to fetch friends.
   - Displays a skeleton loader while loading.
   - Renders each friend as a selectable `<UserItem>`.

4. **Selecting Group Members**  
   - Maintains local state of selected member IDs.
   - Handles toggling of member selection.

5. **Form Submission**  
   - Validates group name and minimum selection of two friends (to make a group of three including self).
   - On success, triggers group creation via an async mutation.
   - Closes the dialog and shows toast notifications for feedback/errors.

6. **Error Handling**  
   - Centralizes error collection and invokes a custom error handler hook for alerts/notifications.

---

## Main Technologies & Hooks Used

- **React**, **Redux**: For state management.
- **MUI**: For UI components and styling.
- **Custom Hooks**: For input validation, async mutation handling, and error handling.
- **API Layer**: For communicating with backend endpoints.

---

## Component Structure

- **Dialog** (modal container)
  - **DialogTitle:** Heading
  - **TextField:** Group name input
  - **Typography:** Section label
  - **Stack:** List of friend items (loading or interactive items)
  - **Action Buttons:** "Cancel" and "Create"

---

## Customizability & Extensibility

- Can be extended to include avatars, search/filter for friends, or other group options.
- Modular enough for easy adaptation to broader chat/group contexts.

---

## User Flow

1. User triggers "Create New Group" (external action).
2. Modal appears; user enters group name.
3. User selects at least two friends to group with.
4. User submits; validations run, and group is created via API.
5. On success, modal closes and feedback is shown. On error, relevant messages are displayed.