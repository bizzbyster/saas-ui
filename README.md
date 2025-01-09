# SaaS Application with AWS Managed Grafana

This repository contains a serverless SaaS application built with Next.js, AWS Managed Grafana, and Clerk for authentication. It features team management, embedded Grafana dashboards, and automated deployment to Vercel.

## Prerequisites

Before you begin, ensure you have the following installed:

- Node.js 18+ and npm
- AWS CLI configured with appropriate permissions
- Vercel CLI
- Git

## Initial Setup

1. Clone the repository:
```bash
git clone https://github.com/bizzbyster/saas-ui.git
cd saas-ui
```

2. Set up Git credentials (one-time setup):
```bash
git config --global credential.helper store
```

3. Configure GitHub authentication:
   - When pushing to GitHub for the first time, you'll be prompted for credentials
   - Username: your GitHub email (e.g., bizzbyster@gmail.com)
   - Password: use a Personal Access Token (not your GitHub password)
   - After entering once, credentials will be stored

4. Install dependencies:
```bash
npm install
```

5. Set up environment variables:
```bash
cp .env.example .env.local
```

Edit `.env.local` and add your credentials:
```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/auth/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/auth/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/dashboard
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/dashboard
NEXT_PUBLIC_GRAFANA_URL=https://your-workspace.grafana-workspace.region.amazonaws.com
NEXT_PUBLIC_GRAFANA_ANONYMOUS_TOKEN=your-token
```

## Local Development

1. Run the development server:
```bash
npm run dev
```

The application will be available at `http://localhost:3000`.

2. Make changes and test locally

3. Commit and push changes:
```bash
git add .
git commit -m "Your commit message"
git push origin main
```

## Production Deployment

### Automated Deployment

This project is configured for automated deployment to Vercel. Every push to the main branch triggers a production deployment.

### Manual Deployment

If needed, you can deploy manually:

```bash
vercel
# or for production
vercel --prod
```

## Domain Configuration

The project uses AWS Route 53 for DNS management. Domain configuration is automated through our setup script.

1. Run the domain setup:
```bash
./setup-saas.sh
```

2. Choose 'y' when prompted for Vercel deployment and domain configuration.

3. Provide your domain name when prompted.

The script will:
- Create/use Route 53 hosted zone
- Configure Vercel domain settings
- Set up necessary DNS records
- Handle SSL certificate provisioning

### Verifying Domain Setup

Check domain configuration:
```bash
vercel domains inspect your-domain.com
```

## Project Structure

```
saas-ui/
├── app/                          # Next.js app directory
│   ├── page.tsx                 # Landing page
│   ├── layout.tsx              # Root layout
│   ├── auth/                   # Authentication pages
│   ├── dashboard/              # Dashboard pages
│   └── team/                   # Team management
├── components/                  # Reusable components
├── middleware.ts               # Auth middleware
├── public/                     # Static assets
└── styles/                     # Global styles
```

## AWS Managed Grafana

### Dashboard Configuration

1. Access your Grafana workspace:
```bash
aws grafana describe-workspace --workspace-id your-workspace-id
```

2. Set up dashboards in Grafana UI:
   - Create visualizations
   - Set permissions
   - Configure anonymous access if needed

### Authentication

The application uses Clerk for user authentication. Grafana access is managed through anonymous tokens for the alpha stage.

## Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build production application
- `npm run start` - Start production server
- `npm run lint` - Run ESLint
- `npm run test` - Run tests (when configured)

## Development Workflow

1. Create feature branch:
```bash
git checkout -b feature/your-feature
```

2. Make changes and test locally

3. Push changes and create PR:
```bash
git push origin feature/your-feature
```

4. Vercel will create a preview deployment for your PR

5. After review and approval, merge to main for production deployment

## Monitoring and Debugging

### Vercel Logs

Access deployment logs:
```bash
vercel logs
```

### Grafana Monitoring

Monitor application metrics through the Grafana dashboard at:
`https://your-workspace.grafana-workspace.region.amazonaws.com`

## Security Considerations

- Environment variables are managed through Vercel
- Clerk handles authentication and authorization
- AWS IAM roles control Grafana access
- SSL certificates are automatically provisioned
- CORS is configured for security

## Contributing

1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

## License

MIT License