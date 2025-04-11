# Real-Time Collaborative Docs

A modern, real-time collaborative document editor built with Next.js, Liveblocks, and Clerk authentication. This project allows multiple users to edit documents simultaneously with real-time updates.

<div align="center">
  <img src="https://img.shields.io/badge/-Next_JS-black?style=for-the-badge&logoColor=white&logo=nextdotjs&color=61DAFB" alt="next.js" />
  <img src="https://img.shields.io/badge/-TypeScript-black?style=for-the-badge&logoColor=white&logo=typescript&color=3178C6" alt="typescript" />
  <img src="https://img.shields.io/badge/-Tailwind_CSS-black?style=for-the-badge&logoColor=white&logo=tailwindcss&color=06B6D4" alt="tailwindcss" />
  <img src="https://img.shields.io/badge/-Liveblocks-black?style=for-the-badge&logoColor=white&logo=liveblocks&color=00F" alt="liveblocks" />
  <img src="https://img.shields.io/badge/-Clerk-black?style=for-the-badge&logoColor=white&logo=clerk&color=000" alt="clerk" />
</div>

## Features

- 🔐 Secure authentication with Clerk
- 🤝 Real-time collaborative editing
- 📝 Rich text editor with Lexical
- 👥 Multi-user presence
- 🎨 Modern UI with Tailwind CSS
- 🛡️ Type-safe with TypeScript
- 📱 Responsive design
- 🚀 Performance optimized

## Prerequisites

Before you begin, ensure you have the following installed:
- Node.js (v18 or later)
- npm or yarn
- Git

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/real-time-docs.git
cd real-time-docs
```

### 2. Install Dependencies

```bash
npm install
# or
yarn install
```

### 3. Environment Setup

Create a `.env` file in the root directory with the following variables:

```env
# Clerk Authentication
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_publishable_key
CLERK_SECRET_KEY=your_secret_key
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/signin
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/signup
NEXT_PUBLIC_CLERK_SIGN_OUT_URL=/signout

# Liveblocks
LIVEBLOCKS_SECRET_KEY=your_secret_key
NEXT_PUBLIC_LIVEBLOCKS_PUBLIC_KEY=your_public_key

# Sentry (Optional)
NEXT_PUBLIC_SENTRY_DSN=your_sentry_dsn
```

### 4. Run the Development Server

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

## Project Structure

```
real-time-docs/
├── app/                  # Next.js app directory
│   ├── (auth)/          # Authentication routes
│   ├── (main)/          # Main application routes
│   └── api/             # API routes
├── components/          # React components
├── lib/                 # Utility functions and configurations
├── public/              # Static assets
├── styles/              # Global styles
└── types/               # TypeScript type definitions
```

## Key Technologies

- **Next.js 14**: React framework for production
- **TypeScript**: Type-safe JavaScript
- **Tailwind CSS**: Utility-first CSS framework
- **Liveblocks**: Real-time collaboration
- **Clerk**: Authentication and user management
- **Lexical**: Rich text editor
- **Sentry**: Error monitoring

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Troubleshooting

### Common Issues

1. **Authentication Issues**
   - Ensure Clerk environment variables are correctly set
   - Check if the Clerk dashboard is properly configured

2. **Real-time Features Not Working**
   - Verify Liveblocks API keys
   - Check network connectivity
   - Ensure proper room creation and access

3. **Build Errors**
   - Clear `.next` directory and node_modules
   - Run `npm install` or `yarn install` again
   - Check TypeScript errors

### Getting Help

If you encounter any issues or have questions:
- Check the [documentation](https://docs.liveblocks.io/)
- Open an issue in the repository
- Join our community Discord

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Liveblocks](https://liveblocks.io/) for the real-time collaboration infrastructure
- [Clerk](https://clerk.dev/) for authentication
- [Next.js](https://nextjs.org/) for the framework
- [Lexical](https://lexical.dev/) for the rich text editor
