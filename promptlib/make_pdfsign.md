
About
The app is a document signing platform that allows users to upload PDF documents, share them for signatures, and manage the signing process. It integrates with Twitter for authentication, enabling users to log in using their Twitter accounts. The app is built using TypeScript and React, leveraging Next.js for server-side rendering and API routes, and utilizes Supabase for database management and file storage.

To use the app, users can log in with their Twitter accounts, upload a PDF document, and share a signing link with recipients. The app provides a dashboard where users can view their uploaded documents, check the signing status, and download signed documents. Users can also sign documents themselves by generating a signature preview and downloading the signed PDF.

Key features of the app include:
- User authentication via Twitter using OAuth 2.0 with PKCE for enhanced security.
- Document upload functionality with validation for file type and size (PDF only, max 10MB).
- A dashboard for managing documents, including viewing, downloading, and sharing signing links.
- Signature generation for both document owners and recipients, with the ability to preview signatures before signing.
- Real-time updates on document signing status, allowing users to see who has signed and when.
- Error handling and user feedback for actions such as uploads and signings.

The app employs various technologies, including:
- TypeScript and React for the frontend.
- Next.js for server-side rendering and API routes.
- Supabase for database and file storage.
- Tailwind CSS for styling and responsive design.
- Crypto and jose libraries for secure token handling and JWT creation.

Overall, the app provides a streamlined and user-friendly experience for electronic document signing, making it easy for users to manage their documents and signatures efficiently.
