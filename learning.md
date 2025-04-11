# Real-Time Docs Project Setup

## Tech Stack

- **React** - For building the web interface
- **Next.js** - Framework to power the application
- **Tailwind CSS** - For styling
- **shadcn/ui** - Component library
- **Liveblocks** - For collaborative experience
- **Clerk** - For authentication
- **Sentry** - For application monitoring

## Project Structure

The project follows a standard Next.js structure with additional components and utilities for real-time collaboration.

## Key Features

- Real-time document editing
- Collaborative workspace
- User authentication
- Modern UI with Tailwind CSS
- Component-based architecture

# Clerk Authentication

Clerk provides a complete authentication and user management solution that's easy to integrate into Next.js applications. Here's how to implement Clerk authentication:

## Setup and Configuration

1. Install Clerk packages:

   ```bash
   npm install @clerk/nextjs
   ```

2. Set up environment variables in `.env`:

   ```
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_publishable_key
   CLERK_SECRET_KEY=your_secret_key
   NEXT_PUBLIC_CLERK_SIGN_IN_URL=/signin
   NEXT_PUBLIC_CLERK_SIGN_UP_URL=/signup
   NEXT_PUBLIC_CLERK_SIGN_OUT_URL=/signout
   ```

3. Middleware:

```
import { clerkMiddleware } from "@clerk/nextjs/server";

export default clerkMiddleware();

export const config = {
  matcher: [
    // Skip Next.js internals and all static files, unless found in search params
    '/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)',
    // Always run for API routes
    '/(api|trpc)(.*)',
  ],
};
```

4. Wrap your application with `ClerkProvider` in `app/layout.tsx`:

   ```tsx
   import { ClerkProvider } from "@clerk/nextjs";
   import { dark } from "@clerk/themes";

   export default function RootLayout({
     children,
   }: {
     children: React.ReactNode;
   }) {
     return (
       <ClerkProvider
         appearance={{
           baseTheme: dark,
           variables: { colorPrimary: "#3371FF" },
         }}
       >
         {/* Your app content */}
       </ClerkProvider>
     );
   }
   ```

# Clerk Authentication Environment Variables

These environment variables are required for Clerk authentication to work properly in your Next.js application. Let me explain each one:

## API Keys
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` and `CLERK_SECRET_KEY`: These are your Clerk API keys that authenticate your application with Clerk's services. The publishable key is used on the client side (hence the `NEXT_PUBLIC_` prefix), while the secret key is used on the server side.

## Authentication URL Variables
The URL variables (`NEXT_PUBLIC_CLERK_SIGN_IN_URL`, `NEXT_PUBLIC_CLERK_SIGN_OUT_URL`, `NEXT_PUBLIC_CLERK_SIGN_UP_URL`) are needed because:
- They tell Clerk where to redirect users after authentication actions
- They ensure consistent routing throughout your application
- They allow Clerk to properly handle authentication flows and protect your routes

For example, when a user clicks "Sign In", Clerk needs to know which page to redirect them to after successful authentication. These environment variables provide that configuration.

# Liveblocks Implementation

## Room Creation and Management

Liveblocks provides real-time collaboration features through rooms. Here's how to implement room creation:

1. Install Liveblocks packages:
   ```bash
   npm install @liveblocks/client @liveblocks/react @liveblocks/react-ui @liveblocks/react-lexical lexical @lexical/react
   ```

2. Set up environment variables in `.env`:
   ```
   LIVEBLOCKS_SECRET_KEY=your_secret_key
   NEXT_PUBLIC_LIVEBLOCKS_PUBLIC_KEY=your_public_key
   ```

3. Create a room with proper metadata and access controls:
   ```typescript
   const createDocument = async ({ userId, email }: CreateDocumentParams) => {
     const roomId = nanoid();
     
     const metadata = {
       creatorId: userId,
       email,
       title: "Untitled Document",
       createdAt: new Date().toISOString(), // Must be string, not Date object
     };

     const usersAccesses: Record<string, ["room:write"]> = {
       [email]: ["room:write"],
     };

     const room = await liveblocks.createRoom(roomId, {
       metadata,
       usersAccesses,
       defaultAccesses: [],
     });
   };
   ```

## Key Learnings

1. **Metadata Types**: 
   - Liveblocks metadata must use string values for dates (use `toISOString()`)
   - All metadata fields must be serializable

2. **Access Control**:
   - Use `usersAccesses` (not `userAccesses`)
   - Access permissions must be tuples with specific values
   - Common permissions: `["room:write"]`, `["room:read"]`, `["room:presence:write"]`

3. **Type Safety**:
   - Use proper TypeScript types for room metadata and access controls
   - Example: `Record<string, ["room:write"]>` for access permissions

4. **Room Management**:
   - Each room needs a unique ID (use `nanoid()`)
   - Rooms can have metadata for tracking creation, ownership, etc.
   - Access controls determine who can read/write to the room

5. **Best Practices**:
   - Always handle errors in room creation
   - Use proper typing for all Liveblocks-related objects
   - Implement proper access control based on user roles
   - Revalidate paths after room creation for Next.js cache management

# Comprehensive Project Learnings and Setup Difficulties

## Project Learnings

### 1. Tech Stack Integration
- Successfully integrated multiple modern technologies:
  - Next.js 14 for the framework
  - TypeScript for type safety
  - Tailwind CSS for styling
  - shadcn/ui for component library
  - Liveblocks for real-time collaboration
  - Clerk for authentication
  - Sentry for error monitoring

### 2. Authentication Implementation
- Learned to implement Clerk authentication with:
  - Proper environment variable setup
  - Middleware configuration
  - Provider wrapping in the application
  - Custom theme configuration
  - Secure routing protection

### 3. Real-time Collaboration
- Gained expertise in Liveblocks implementation:
  - Room creation and management
  - Access control configuration
  - Metadata handling
  - Real-time presence features
  - Proper typing for collaborative features

### 4. Editor Implementation
- Successfully integrated Lexical editor with:
  - Real-time collaboration features
  - Custom plugin development
  - Proper state management
  - Type-safe implementation

### 5. Project Structure
- Implemented a well-organized Next.js project structure:
  - Clear separation of concerns
  - Type definitions in separate directory
  - Component-based architecture
  - Proper configuration files organization

## Setup Difficulties Faced

### 1. Environment Configuration
- Initial challenges with environment variables setup:
  - Multiple API keys to manage (Clerk, Liveblocks)
  - Proper configuration of public vs private keys
  - Ensuring secure handling of sensitive keys

### 2. Authentication Flow
- Complexities in implementing Clerk:
  - Setting up proper middleware
  - Configuring redirect URLs
  - Handling authentication states
  - Managing protected routes

### 3. Real-time Features
- Challenges with Liveblocks integration:
  - Room creation and management
  - Access control configuration
  - Proper typing of metadata
  - Handling real-time updates
  - Managing user presence

### 4. Editor Integration
- Difficulties in implementing Lexical editor:
  - Integration with Liveblocks
  - Custom plugin development
  - State synchronization
  - Performance optimization

### 5. Type Safety
- TypeScript implementation challenges:
  - Proper typing of collaborative features
  - Type definitions for external libraries
  - Ensuring type safety across components

### 6. Build and Deployment
- Configuration challenges:
  - Next.js build optimization
  - Proper configuration of Sentry
  - Environment-specific settings
  - Production deployment considerations

### 7. Performance Optimization
- Challenges in maintaining performance:
  - Real-time updates optimization
  - Editor performance
  - State management efficiency
  - Proper caching strategies

## Key Success Factors

The project successfully overcame these challenges through:
- Proper documentation
- Clear project structure
- Type-safe implementation
- Comprehensive error handling
- Performance optimization
- Security best practices

This project serves as an excellent example of building a modern, real-time collaborative application with proper security, performance, and user experience considerations.

# Room Management and Real-time Updates

## Room Creation and Management

### Creating a Room
```typescript
// Example of room creation
const createRoom = async (userId: string, email: string) => {
  const roomId = nanoid(); // Generate unique room ID
  
  const metadata = {
    creatorId: userId,
    email,
    title: "Untitled Document",
    createdAt: new Date().toISOString(),
  };

  const usersAccesses: Record<string, ["room:write"]> = {
    [email]: ["room:write"],
  };

  const room = await liveblocks.createRoom(roomId, {
    metadata,
    usersAccesses,
    defaultAccesses: [],
  });
};
```

### Room Management Best Practices
- Always generate unique room IDs using `nanoid()`
- Store room metadata as serializable data (strings, numbers, booleans)
- Implement proper error handling for room creation
- Clean up unused rooms to optimize resources
- Track room creation time and last activity

## Access Control Configuration

### Types of Access Permissions
- `["room:write"]` - Allows users to modify room content
- `["room:read"]` - Allows users to view room content
- `["room:presence:write"]` - Allows users to update their presence
- `["room:metadata:write"]` - Allows users to modify room metadata

### Implementing Access Control
```typescript
// Example of access control configuration
const accessControl = {
  // User-specific access
  usersAccesses: {
    "user1@example.com": ["room:write"],
    "user2@example.com": ["room:read"],
  },
  
  // Default access for all users
  defaultAccesses: ["room:read"],
  
  // Group-based access
  groupsAccesses: {
    "editors": ["room:write"],
    "viewers": ["room:read"],
  }
};
```

### Access Control Best Practices
- Use granular permissions for better security
- Implement role-based access control
- Regularly review and update access permissions
- Log access changes for audit purposes
- Handle access revocation properly

## Real-time Updates Handling

### Presence Updates
```typescript
// Example of presence handling
const { presence } = useMyPresence();

// Update user presence
presence.update({
  cursor: { x: 100, y: 200 },
  selection: null,
  user: {
    name: "John Doe",
    color: "#ff0000"
  }
});
```

### Document Updates
```typescript
// Example of document updates
const { room } = useRoom();

// Subscribe to document changes
room.subscribe("document", (document) => {
  // Handle document updates
  console.log("Document updated:", document);
});

// Update document
room.updateDocument({
  content: "New content",
  lastModified: new Date().toISOString()
});
```

### Real-time Update Best Practices
- Implement proper error handling for updates
- Use optimistic updates for better UX
- Handle conflicts gracefully
- Implement proper state synchronization
- Monitor update performance
- Use proper typing for all update operations

## Key Implementation Considerations

### Performance Optimization
- Batch updates when possible
- Implement proper debouncing for frequent updates
- Use efficient data structures for updates
- Monitor network usage
- Implement proper caching strategies

### Error Handling
- Implement proper error boundaries
- Handle network disconnections
- Provide fallback mechanisms
- Log errors for debugging
- Implement retry mechanisms

### Security Considerations
- Validate all updates on the server
- Implement proper access control
- Sanitize user input
- Monitor for suspicious activities
- Implement rate limiting

# Detailed Implementation Process

## 1. Document Editor Page Implementation

### Setup and Structure
```typescript
// pages/document/[documentId].tsx
const DocumentPage = () => {
  const { documentId } = useParams();
  const { user } = useUser();
  
  return (
    <div className="document-container">
      <DocumentHeader />
      <Editor documentId={documentId} />
      <CommentsPanel />
    </div>
  );
};
```

### Key Components
- Document header with title and sharing options
- Rich text editor integration
- Real-time collaboration features
- Comments panel
- User presence indicators

### Editor Configuration
- Integration with Lexical editor
- Custom plugins for formatting
- Real-time sync with Liveblocks
- Auto-save functionality

## 2. Clerk Authentication Implementation

### Setup Process
1. Installation and configuration:
```bash
npm install @clerk/nextjs
```

2. Environment configuration:
```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_****
CLERK_SECRET_KEY=sk_****
```

### Authentication Flow
1. User sign-in/sign-up process
2. Protected route handling
3. User session management
4. Authentication state persistence

### Integration with Next.js
```typescript
// app/layout.tsx
import { ClerkProvider } from "@clerk/nextjs";

export default function RootLayout({ children }) {
  return (
    <ClerkProvider>
      <html>
        <body>{children}</body>
      </html>
    </ClerkProvider>
  );
}
```

## 3. Liveblocks Integration with Authentication

### Setup and Configuration
```typescript
// liveblocks.config.ts
import { createClient } from "@liveblocks/client";

const client = createClient({
  publicApiKey: process.env.NEXT_PUBLIC_LIVEBLOCKS_PUBLIC_KEY!,
  async authEndpoint(room) {
    const response = await fetch("/api/liveblocks-auth", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ room }),
    });
    return response.json();
  },
});
```

### User Authentication Flow
1. User authentication with Clerk
2. Liveblocks session creation
3. Room access verification
4. Real-time presence management

## 4. Collaborative Room Creation

### Room Creation Process
```typescript
const createCollaborativeRoom = async ({ userId, title }: RoomParams) => {
  const roomId = nanoid();
  
  const room = await liveblocks.createRoom(roomId, {
    metadata: {
      createdBy: userId,
      title,
      createdAt: new Date().toISOString(),
    },
    defaultAccesses: [],
    usersAccesses: {
      [userId]: ["room:write"],
    },
  });

  return room;
};
```

### Room Management
- Unique room identification
- Access control setup
- Metadata management
- User permissions

## 5. Document Editing Implementation

### Editor Setup
```typescript
const DocumentEditor = ({ documentId }: EditorProps) => {
  const { isLoading, data } = useDocument(documentId);
  
  return (
    <LiveblocksProvider>
      <Editor
        onChange={handleChange}
        initialContent={data?.content}
        collaborative
      />
    </LiveblocksProvider>
  );
};
```

### Real-time Collaboration Features
- Cursor presence
- Selection sync
- Conflict resolution
- Auto-save mechanism

## 6. Document Listing Implementation

### Document List Component
```typescript
const DocumentList = () => {
  const { documents, isLoading } = useDocuments();
  
  return (
    <div className="document-grid">
      {documents.map(doc => (
        <DocumentCard
          key={doc.id}
          document={doc}
          onOpen={handleOpen}
          onDelete={handleDelete}
        />
      ))}
    </div>
  );
};
```

### Features
- Grid/List view toggle
- Sort and filter options
- Search functionality
- Document previews

## 7. Live Comments Feature

### Comments Implementation
```typescript
const CommentsPanel = ({ documentId }: CommentsPanelProps) => {
  const { comments, addComment } = useComments(documentId);
  
  const handleAddComment = async (content: string) => {
    await addComment({
      content,
      documentId,
      createdAt: new Date().toISOString(),
    });
  };
  
  return (
    <div className="comments-panel">
      <CommentsList comments={comments} />
      <CommentInput onSubmit={handleAddComment} />
    </div>
  );
};
```

### Features
- Real-time updates
- Thread support
- Markdown formatting
- File attachments

## 8. User Mention Feature

### Mention Implementation
```typescript
const MentionPlugin = () => {
  const [suggestions, setSuggestions] = useState([]);
  
  const handleSearch = async (query: string) => {
    const users = await searchUsers(query);
    setSuggestions(users);
  };
  
  return (
    <MentionsPlugin
      onSearch={handleSearch}
      suggestions={suggestions}
      renderSuggestion={UserSuggestion}
    />
  );
};
```

### Features
- User search
- Mention suggestions
- Notification triggers
- Mention highlighting

## 9. Document Sharing Implementation

### Sharing Component
```typescript
const ShareDocument = ({ documentId }: ShareProps) => {
  const { users, permissions } = useDocumentPermissions(documentId);
  
  const handleShare = async (email: string, permission: Permission) => {
    await updatePermissions(documentId, {
      [email]: permission,
    });
  };
  
  return (
    <div className="share-modal">
      <UserSearch onSelect={handleShare} />
      <PermissionsList users={users} permissions={permissions} />
    </div>
  );
};
```

### Features
- Email invitation
- Permission levels
- Link sharing
- Access management

## 10. Permission Management

### Adding Permissions
```typescript
const addPermission = async (documentId: string, userId: string, permission: Permission) => {
  await liveblocks.updateRoom(documentId, {
    usersAccesses: {
      [userId]: [permission],
    },
  });
};
```

### Removing Permissions
```typescript
const removePermission = async (documentId: string, userId: string) => {
  await liveblocks.updateRoom(documentId, {
    usersAccesses: {
      [userId]: [],
    },
  });
};
```

## 11. Document Deletion

### Deletion Implementation
```typescript
const deleteDocument = async (documentId: string) => {
  // Remove from database
  await db.documents.delete(documentId);
  
  // Delete Liveblocks room
  await liveblocks.deleteRoom(documentId);
  
  // Clean up associated resources
  await cleanup(documentId);
};
```

### Features
- Soft deletion option
- Permission verification
- Resource cleanup
- Deletion confirmation

## 12. Notification System

### Notification Types
```typescript
type NotificationType =
  | "DOCUMENT_SHARED"
  | "COMMENT_ADDED"
  | "MENTION"
  | "PERMISSION_CHANGED"
  | "DOCUMENT_DELETED";
```

### Implementation
```typescript
const NotificationSystem = () => {
  const { notifications } = useNotifications();
  
  const handleNotification = async (notification: Notification) => {
    switch (notification.type) {
      case "DOCUMENT_SHARED":
        // Handle document sharing notification
        break;
      case "COMMENT_ADDED":
        // Handle new comment notification
        break;
      case "MENTION":
        // Handle mention notification
        break;
      // ... handle other cases
    }
  };
  
  return (
    <NotificationProvider onNotification={handleNotification}>
      <NotificationList notifications={notifications} />
    </NotificationProvider>
  );
};
```

### Features
- Real-time notifications
- Email notifications
- Notification preferences
- Read/unread status
- Notification grouping

## Best Practices and Learnings

### Performance Optimization
- Implement proper caching strategies
- Use optimistic updates
- Batch operations when possible
- Lazy load components

### Security Considerations
- Validate all user inputs
- Implement proper access control
- Handle sensitive data securely
- Rate limit API requests

### Error Handling
- Implement proper error boundaries
- Provide meaningful error messages
- Handle edge cases gracefully
- Implement retry mechanisms

### User Experience
- Provide loading states
- Implement proper feedback
- Ensure responsive design
- Maintain consistency

This implementation provides a robust foundation for a collaborative document editing platform with real-time features, secure authentication, and comprehensive user management.
