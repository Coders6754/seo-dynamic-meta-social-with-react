# 🚀 React SSR with Dynamic Meta Tags

A production-ready **Server-Side Rendered (SSR)** React application with **dynamic Open Graph (OG) tags** and **Twitter Cards** for perfect social media previews on WhatsApp, Twitter, Facebook, and more.

## ✨ Features

- ⚡ **Server-Side Rendering** with React 18 and Vite
- 🎯 **Dynamic Meta Tags** - Title, description, and images change per route
- 📱 **Social Media Ready** - Perfect previews for WhatsApp, Twitter, Facebook, LinkedIn
- 🔄 **Hydration** - Seamless client-side navigation after initial SSR
- 🎨 **React Router** - Full routing support with SSR
- 🏗️ **Production Build** - Optimized Vite configuration for SSR
- 📦 **ESM Support** - Modern ES modules throughout

## 🎯 Why This Matters

When you share a link like `yoursite.com/post/123` on WhatsApp or Twitter, the platform crawls your page and displays a preview card. Without SSR and proper meta tags, you'd see:
- ❌ Generic title and description
- ❌ Missing or default image
- ❌ No dynamic content based on the route

With this setup, you get:
- ✅ **Dynamic titles** based on the route (e.g., "Post #123 - Amazing Content 🚀")
- ✅ **Unique descriptions** for each page
- ✅ **Route-specific images** for rich previews
- ✅ **Perfect social media cards** every time

## 🛠️ Tech Stack

- **React 18** - UI library with SSR support
- **Vite 5** - Fast build tool and dev server
- **Express** - Node.js server framework
- **React Router 6** - Client and server-side routing
- **ESM** - Modern JavaScript modules

## 📦 Installation

```bash
# Navigate to the frontend directory
cd frontend

# Install dependencies
npm install

# Build the project
npm run build

# Start the server
node server.js
```

The server will start on `http://localhost:3000`

## 🚀 Quick Start

### Development

```bash
npm run dev
```

### Production Build

```bash
npm run build
npm run preview
```

### Scripts

- `npm run dev` - Start the development server
- `npm run build` - Build for production
- `npm run preview` - Build and start production server

## 📁 Project Structure

```
frontend/
├── src/
│   ├── entry-client.jsx    # Client-side hydration entry
│   ├── entry-server.jsx    # Server-side rendering entry
│   ├── App.jsx             # Main app component
│   ├── Home.jsx            # Home page component
│   └── Post.jsx            # Dynamic post page component
├── server.js               # Express server with SSR
├── vite.config.js          # Vite configuration for SSR
├── index.html              # HTML template
└── dist/                   # Build output
    ├── client/             # Client bundle
    └── server/             # Server bundle
```

## 🔧 How It Works

### 1. Server-Side Rendering

When a request comes in:
1. Express server receives the request
2. Extracts route data (e.g., post ID from `/post/123`)
3. Generates dynamic meta tags (title, description, image)
4. Renders React app on the server with `renderToPipeableStream`
5. Injects meta tags into HTML `<head>`
6. Streams the response to the client

### 2. Client-Side Hydration

After the initial SSR:
1. React hydrates the server-rendered HTML
2. Client-side routing takes over
3. Navigation happens without full page reloads
4. Meta tags update dynamically (client-side only)

### 3. Dynamic Meta Tags

The `getPageData()` function in `server.js` generates route-specific metadata:

```javascript
function getPageData(url) {
  if (url.startsWith('/post/')) {
    const id = url.split('/')[2] || '123';
    return {
      title: `Post #${id} - Amazing Content 🚀`,
      description: `Check out this amazing post #${id}!`,
      image: `https://picsum.photos/seed/post${id}/1200/630`,
    };
  }
  // Default home page data
  return { ... };
}
```

## 🎨 Usage Examples

### Adding a New Route

1. **Create a component** in `src/`:

```jsx
// src/Product.jsx
export default function Product({ data }) {
  return (
    <div>
      <h1>{data.title}</h1>
      <p>{data.description}</p>
    </div>
  );
}
```

2. **Add route** in `src/App.jsx`:

```jsx
<Route path="/product/:id" element={<Product data={initialData} />} />
```

3. **Add page data** in `server.js`:

```javascript
function getPageData(url) {
  if (url.startsWith('/product/')) {
    const id = url.split('/')[2];
    return {
      title: `Product ${id} - Buy Now!`,
      description: `Amazing product ${id} available now!`,
      image: `https://yoursite.com/images/product-${id}.jpg`,
    };
  }
  // ... other routes
}
```

### Customizing Meta Tags

Edit the `metaTags` template in `server.js`:

```javascript
const metaTags = `
  <title>${pageData.title}</title>
  <meta name="description" content="${pageData.description}">
  
  <!-- Open Graph / Facebook -->
  <meta property="og:title" content="${pageData.title}">
  <meta property="og:description" content="${pageData.description}">
  <meta property="og:image" content="${pageData.image}">
  
  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="${pageData.title}">
  <!-- ... -->
`;
```

## 🌐 Deployment

### Environment Variables

Set the `PORT` environment variable (defaults to 3000):

```bash
PORT=8080 node server.js
```

### Production Considerations

1. **Use a reverse proxy** (nginx, Cloudflare) for HTTPS
2. **Set proper CORS** origins in production
3. **Use CDN** for static assets
4. **Enable gzip compression**
5. **Set up proper caching headers**

### Example Deployment (Vercel/Netlify)

For serverless deployment, you'll need to adapt the Express server to serverless functions. The current setup works best with:
- VPS (DigitalOcean, AWS EC2)
- Railway
- Render
- Heroku

## 🧪 Testing Social Previews

### Local Testing

1. **Use ngrok** to expose your local server:
   ```bash
   ngrok http 3000
   ```

2. **Test with social media validators**:
   - [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
   - [Twitter Card Validator](https://cards-dev.twitter.com/validator)
   - [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)

3. **Test on WhatsApp**:
   - Share the ngrok URL in a WhatsApp chat
   - Verify the preview card shows correctly

### Test URLs

- Home: `http://localhost:3000/`
- Post 123: `http://localhost:3000/post/123`
- Post 456: `http://localhost:3000/post/456`

## 📝 Key Configuration

### Vite SSR Config

The `vite.config.js` is configured to:
- ✅ Build separate client and server bundles
- ✅ Externalize Node.js dependencies for server
- ✅ Preserve exports for server entry
- ✅ Use ESM format throughout

### Express Server

The `server.js` handles:
- ✅ Route detection and data extraction
- ✅ Dynamic meta tag generation
- ✅ React SSR streaming
- ✅ HTML template injection
- ✅ Static asset serving

## 🐛 Troubleshooting

### Build Errors

**Issue**: `renderToPipeableStream is not a function`
- **Solution**: Ensure `react-dom/server` is externalized in `vite.config.js`

**Issue**: `Cannot find module 'react-router-dom/server'`
- **Solution**: Use `react-router-dom/server.js` with `.js` extension

### Runtime Errors

**Issue**: Meta tags not updating
- **Solution**: Clear browser cache and check server logs

**Issue**: Social previews not working
- **Solution**: 
  1. Verify meta tags in HTML source
  2. Use social media validators to test
  3. Ensure images are publicly accessible
  4. Check that URLs are absolute (not relative)

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests

## 📄 License

MIT License - feel free to use this project for your own applications!

## 🙏 Acknowledgments

- Built with [React](https://react.dev/)
- Powered by [Vite](https://vitejs.dev/)
- Server-side rendering with [Express](https://expressjs.com/)

---

**Made with ❤️ for perfect social media previews**

For questions or issues, please open an issue on GitHub.

