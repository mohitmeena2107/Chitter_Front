# ChatList Component - High-Level Documentation

## Overview

The `ChatList` component is a React functional component that displays a vertical list of chat conversations, typically used in chat or messaging applications. It renders each chat as a `ChatItem` and supports online status, new message alerts, chat deletion, and customization of layout.

---

## Props

- **w** (string, optional): Width of the chat list container. Defaults to `"100%"`.
- **chats** (array, optional): List of chat objects to display. Each chat contains properties such as `_id`, `avatar`, `name`, `groupChat`, and `members`.
- **chatId** (string, required): The currently active or selected chat ID.
- **onlineUsers** (array, optional): List of user IDs currently online to help indicate presence in group chats.
- **newMessagesAlert** (array, optional): Objects containing `{ chatId, count }` to indicate unread message counts for specific chats.
- **handleDeleteChat** (function, optional): Handler function to be called when a user wants to delete a chat.

---

## Functionality

- **Rendering Chats**: Maps over the `chats` array and renders a `ChatItem` for each chat entry.
- **Online Status**: Checks if any member of the chat is present in the `onlineUsers` array and passes this status to each `ChatItem`.
- **New Message Alerts**: Passes the count and presence of new messages for each chat to the relevant chat item.
- **Active Chat Highlighting**: Marks the chat visually if it is the currently selected (open) chat.
- **Chat Deletion Support**: For each chat, enables the ability to delete via the passed handler.
- **Styling/Layout**: Uses a Material UI `Stack` to create a vertically scrollable area that displays chat items as a column.

---

## Usage

The `ChatList` component is typically included as a sidebar or main panel in a chat application. It relies on separate `ChatItem` components for each list entry, handling UI and user interaction per chat item (such as selecting, seeing online status, and new message indicators).

---

## Dependencies

- React
- Material UI (Stack component)
- Custom `ChatItem` component

---

## Summary

`ChatList` provides a dynamic, user-friendly list of chat overviews with live updates for online status and unread messages, supporting key chat application features like active-chat highlighting and chat removal. Designed to be reusable and customizable for various chat-based UIs.