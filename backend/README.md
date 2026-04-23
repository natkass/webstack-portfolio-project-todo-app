# Backend Service

This is the backend service for the application. It is built using the following tech stack:

- NodeJS
- TypeScript
- TypeORM (MySQL)

### Requirements
- NodeJS v18+
- MySQL 8+

### Depends On
- MySQL Database


### Environment Variables
The application requires the following environment variables to be set:
- `DB_USER`: The username to connect to the MySQL database.
- `DB_PASSWORD`: The password to connect to the MySQL database.
- `DB_NAME`: The name of the MySQL database.
- `FRONTEND_API_KEY (optional)`: The API key for frontend-backend communication. Must be the same as in the frontend service.


### How to run

Install Dependencies
```
npm ci
```

Run in development mode
```
npm run dev
```

Build for production
```
npm run build
```

Run in production mode
```
npm start
```