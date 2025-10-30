# React Shortcuts & Commands Cheat Sheet

## 🚀 npm/yarn Commands

### Project Setup
```bash
npm create vite@latest          # Create new Vite + React project
npm install                     # Install dependencies (alias: npm i)
npm run dev                     # Start development server
npm run build                   # Build for production
npm run preview                 # Preview production build
npm run lint                    # Run linter
```

### Package Management
```bash
npm install <package>           # Install a package
npm install -D <package>        # Install as dev dependency
npm uninstall <package>         # Remove a package
npm update                      # Update packages
npm outdated                    # Check outdated packages
```

## ⚛️ React Code Snippets (VS Code with ES7+ React/Redux extension)

### Component Snippets
- `rafce` - React Arrow Function Component with Export
- `rfc` - React Functional Component
- `rfce` - React Functional Component with Export
- `rafc` - React Arrow Function Component
- `rcc` - React Class Component
- `rce` - React Class Component with Export

### Hooks
- `useState` - useState hook
- `useEffect` - useEffect hook
- `useContext` - useContext hook
- `useReducer` - useReducer hook
- `useCallback` - useCallback hook
- `useMemo` - useMemo hook
- `useRef` - useRef hook

### Import Shortcuts
- `imr` - Import React
- `imrc` - Import React + Component
- `imrs` - Import React + useState
- `imrse` - Import React + useState + useEffect

## ⌨️ VS Code Shortcuts

### General
- `Ctrl + Space` - Trigger IntelliSense
- `Ctrl + .` - Quick fix / Auto-import
- `F2` - Rename symbol
- `Ctrl + Shift + F` - Search in all files
- `Ctrl + P` - Quick open file
- `Ctrl + Shift + P` - Command palette

### Editing
- `Alt + Up/Down` - Move line up/down
- `Ctrl + D` - Select next occurrence
- `Ctrl + /` - Toggle comment
- `Shift + Alt + F` - Format document
- `Ctrl + Shift + K` - Delete line
- `Ctrl + Shift + L` - Select all occurrences
- `Ctrl + [` / `Ctrl + ]` - Indent/Outdent line

### Navigation
- `Ctrl + B` - Toggle sidebar
- `Ctrl + \`` - Toggle terminal
- `Alt + ←/→` - Go back/forward
- `Ctrl + Tab` - Switch between files

## 📦 Common React Packages to Install

### Routing
```bash
npm install react-router-dom
```

### State Management
```bash
npm install redux react-redux           # Redux
npm install zustand                      # Zustand (lightweight)
npm install @tanstack/react-query        # React Query (server state)
```

### Forms
```bash
npm install react-hook-form              # React Hook Form
npm install formik yup                   # Formik + Yup validation
```

### UI Libraries
```bash
npm install @mui/material @emotion/react @emotion/styled    # Material-UI
npm install antd                                             # Ant Design
npm install tailwindcss postcss autoprefixer                # Tailwind CSS
npm install shadcn-ui                                        # shadcn/ui
```

### HTTP Requests
```bash
npm install axios                        # Axios
npm install swr                          # SWR (data fetching)
```

### Icons
```bash
npm install react-icons                  # React Icons
npm install lucide-react                 # Lucide Icons
npm install @heroicons/react             # Heroicons
```

### Testing
```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

## 🔥 Useful React Patterns (Quick Templates)

### Basic Component with State
```javascript
import { useState } from 'react'

function MyComponent() {
  const [state, setState] = useState(initialValue)
  
  const handleClick = () => {
    setState(newValue)
  }
  
  return (
    <div>
      <p>{state}</p>
      <button onClick={handleClick}>Update</button>
    </div>
  )
}

export default MyComponent
```

### Component with useEffect
```javascript
import { useState, useEffect } from 'react'

function MyComponent() {
  const [data, setData] = useState(null)
  
  useEffect(() => {
    // Side effect here (runs after render)
    console.log('Component mounted or updated')
    
    return () => {
      // Cleanup function (runs before unmount)
      console.log('Component will unmount')
    }
  }, []) // Empty array = run once on mount
  
  return <div>{data}</div>
}

export default MyComponent
```

### Fetch Data Pattern
```javascript
import { useState, useEffect } from 'react'

function DataFetcher() {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetch('/api/endpoint')
      .then(res => {
        if (!res.ok) throw new Error('Failed to fetch')
        return res.json()
      })
      .then(data => setData(data))
      .catch(err => setError(err.message))
      .finally(() => setLoading(false))
  }, [])

  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>
  if (!data) return <div>No data</div>

  return <div>{JSON.stringify(data)}</div>
}

export default DataFetcher
```

### Custom Hook Pattern
```javascript
import { useState, useEffect } from 'react'

function useFetch(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false))
  }, [url])

  return { data, loading, error }
}

// Usage:
function MyComponent() {
  const { data, loading, error } = useFetch('/api/data')
  
  if (loading) return <div>Loading...</div>
  if (error) return <div>Error!</div>
  
  return <div>{data}</div>
}
```

### Context API Pattern
```javascript
import { createContext, useContext, useState } from 'react'

// Create context
const ThemeContext = createContext()

// Provider component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light')
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

// Custom hook to use the context
export function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider')
  }
  return context
}

// Usage in component:
function MyComponent() {
  const { theme, toggleTheme } = useTheme()
  return <button onClick={toggleTheme}>{theme}</button>
}
```

### Form Handling Pattern
```javascript
import { useState } from 'react'

function FormComponent() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  })

  const handleChange = (e) => {
    const { name, value } = e.target
    setFormData(prev => ({
      ...prev,
      [name]: value
    }))
  }

  const handleSubmit = (e) => {
    e.preventDefault()
    console.log('Form submitted:', formData)
    // Send data to server
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      <button type="submit">Submit</button>
    </form>
  )
}

export default FormComponent
```

## 🛠️ Git Commands (for React projects)

```bash
# Initialize repository
git init

# Stage all files
git add .

# Commit changes
git commit -m "Initial commit"

# Rename branch to main
git branch -M main

# Add remote repository
git remote add origin <repository-url>

# Push to remote
git push -u origin main

# Common workflow
git status                    # Check status
git add <file>                # Stage specific file
git commit -m "message"       # Commit with message
git pull                      # Pull latest changes
git push                      # Push changes
```

## 🎨 React Best Practices

1. **Component Organization**
   - One component per file
   - Name files with PascalCase
   - Keep components small and focused

2. **State Management**
   - Use local state when possible
   - Lift state up when needed
   - Consider context for global state
   - Use Redux/Zustand for complex state

3. **Performance**
   - Use `React.memo()` for expensive components
   - Use `useMemo()` for expensive calculations
   - Use `useCallback()` for stable function references
   - Lazy load components with `React.lazy()`

4. **Code Style**
   - Use destructuring for props
   - Use arrow functions for components
   - Keep JSX readable with proper formatting
   - Use meaningful variable names

## 📚 Useful Resources

- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [React Router](https://reactrouter.com/)
- [React Query](https://tanstack.com/query/latest)
- [React Hook Form](https://react-hook-form.com/)

---

**Last Updated:** October 30, 2025
