<template>
  <div class="login-page">
    <!-- Animated background blobs -->
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>

    <div class="login-container">
      <!-- Left: Branding Panel -->
      <div class="login-left">
        <div class="brand">
          <div class="brand-icon">
            <img src="https://sysinfra.in/img/mobile-logo.png" alt="Logo" width="32" height="32" style="object-fit: contain;" />
          </div>
          <span class="brand-name">System Infra Solutions</span>
        </div>

        <div class="left-content">
          <h1>Welcome back to your workspace</h1>
          <p>Manage your team, track performance, and stay on top of your goals — all in one place.</p>
          <div class="feature-list">
            <div class="feature-item" v-for="item in features" :key="item.text">
              <div class="feature-icon">{{ item.icon }}</div>
              <span>{{ item.text }}</span>
            </div>
          </div>
        </div>

        <div class="left-footer">
          <div class="avatar-group">
            <div class="avatar" v-for="i in 4" :key="i" :style="{ background: avatarColors[i-1] }">{{ avatarLetters[i-1] }}</div>
          </div>
          <span>Join <strong>2,400+</strong> users already on System Infra Solutions</span>
        </div>
      </div>

      <!-- Right: Login Form -->
      <div class="login-right">
        <div class="login-card">
          <div class="login-header">
            <h2>Sign in</h2>
            <p>Enter your credentials to access your account</p>
          </div>

          <form @submit.prevent="handleLogin" class="login-form">
            <div class="form-group" :class="{ error: errors.email }">
              <label for="email">Email address</label>
              <div class="input-wrapper">
                <span class="input-icon">
                  <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
                  </svg>
                </span>
                <input id="email" v-model="form.email" type="email" placeholder="you@example.com" autocomplete="email" />
              </div>
              <span class="error-msg" v-if="errors.email">{{ errors.email }}</span>
            </div>

            <div class="form-group" :class="{ error: errors.password }">
              <label for="password">Password</label>
              <div class="input-wrapper">
                <span class="input-icon">
                  <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/>
                  </svg>
                </span>
                <input id="password" v-model="form.password" :type="showPassword ? 'text' : 'password'" placeholder="Enter password" autocomplete="current-password" />
                <button type="button" class="toggle-password" @click="showPassword = !showPassword">
                  <svg v-if="!showPassword" width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/>
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/>
                  </svg>
                  <svg v-else width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21"/>
                  </svg>
                </button>
              </div>
              <span class="error-msg" v-if="errors.password">{{ errors.password }}</span>
            </div>

            <div class="form-options">
              <label class="checkbox-label">
                <input type="checkbox" v-model="form.remember" />
                <span class="checkbox-custom"></span>
                Remember me
              </label>
              <a href="#" class="forgot-link">Forgot password?</a>
            </div>

            <div class="login-error" v-if="loginError">
              <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
              </svg>
              {{ loginError }}
            </div>

            <button type="submit" class="btn btn-primary submit-btn" :disabled="loading">
              <svg v-if="loading" class="spinner" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
              </svg>
              <span>{{ loading ? 'Signing in...' : 'Sign in' }}</span>
              <svg v-if="!loading" width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
              </svg>
            </button>

            <div class="demo-hint">
              <span>Demo:</span> email: <code>admin@sysinfra.in</code> &nbsp; pass: <code>1234</code>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const loading = ref(false)
const loginError = ref('')
const showPassword = ref(false)

const form = reactive({
  email: '',
  password: '',
  remember: false
})

const errors = reactive({
  email: '',
  password: ''
})

const features = [
  { icon: '📊', text: 'Real-time analytics & reporting' },
  { icon: '👥', text: 'Team collaboration tools' },
  { icon: '🔒', text: 'Enterprise-grade security' },
  { icon: '⚡', text: 'Lightning fast performance' }
]

const avatarColors = ['#6C63FF', '#06b6d4', '#10b981', '#f59e0b']
const avatarLetters = ['R', 'B', 'C', 'D']

function validate() {
  errors.email = ''
  errors.password = ''
  let valid = true
  if (!form.email) { errors.email = 'Email is required'; valid = false }
  else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) { errors.email = 'Enter a valid email'; valid = false }
  if (!form.password) { errors.password = 'Password is required'; valid = false }
  else if (form.password.length < 4) { errors.password = 'At least 4 characters'; valid = false }
  return valid
}

async function handleLogin() {
  loginError.value = ''
  if (!validate()) return
  loading.value = true
  await new Promise(r => setTimeout(r, 1200))
  if (form.email === 'admin@sysinfra.in' && form.password === '1234') {
    localStorage.setItem('auth_token', 'demo-token-123')
    localStorage.setItem('user_name', 'Rajesh')
    localStorage.setItem('user_email', form.email)
    router.push('/dashboard')
  } else {
    loginError.value = 'Invalid email or password. Try the demo credentials.'
  }
  loading.value = false
}
</script>

<style scoped>
.login-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg-dark);
  position: relative;
  overflow: hidden;
  padding: 20px;
}

/* Animated blobs */
.blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.35;
  animation: float 8s ease-in-out infinite;
  pointer-events: none;
}
.blob-1 { width: 500px; height: 500px; background: radial-gradient(circle, #6C63FF, transparent); top: -150px; left: -150px; animation-delay: 0s; }
.blob-2 { width: 400px; height: 400px; background: radial-gradient(circle, #06b6d4, transparent); bottom: -100px; right: -100px; animation-delay: 2s; }
.blob-3 { width: 300px; height: 300px; background: radial-gradient(circle, #10b981, transparent); top: 50%; left: 50%; transform: translate(-50%,-50%); animation-delay: 4s; }

@keyframes float {
  0%, 100% { transform: scale(1) translate(0,0); }
  50% { transform: scale(1.08) translate(15px, -15px); }
}

.login-container {
  display: flex;
  width: 100%;
  max-width: 1000px;
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 40px 100px rgba(0,0,0,0.5);
  z-index: 1;
  position: relative;
}

/* Left panel */
.login-left {
  flex: 1;
  background: linear-gradient(135deg, #1a1a3e 0%, #0d1117 100%);
  padding: 50px 44px;
  display: flex;
  flex-direction: column;
  border-right: 1px solid var(--border);
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 60px;
}
.brand-name {
  font-size: 20px;
  font-weight: 700;
  color: var(--text-primary);
  letter-spacing: -0.3px;
}

.left-content h1 {
  font-size: 28px;
  font-weight: 800;
  line-height: 1.3;
  color: var(--text-primary);
  margin-bottom: 14px;
  letter-spacing: -0.5px;
}
.left-content p {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 36px;
}

.feature-list { display: flex; flex-direction: column; gap: 14px; }
.feature-item {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 14px;
  color: var(--text-secondary);
}
.feature-icon {
  width: 38px; height: 38px;
  border-radius: 10px;
  background: rgba(108,99,255,0.15);
  display: flex; align-items: center; justify-content: center;
  font-size: 18px;
  flex-shrink: 0;
}

.left-footer {
  margin-top: auto;
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
  color: var(--text-secondary);
}
.avatar-group { display: flex; }
.avatar {
  width: 32px; height: 32px;
  border-radius: 50%;
  color: #fff;
  font-size: 12px;
  font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  margin-right: -8px;
  border: 2px solid var(--bg-card2);
}

/* Right panel (form) */
.login-right {
  flex: 1;
  background: var(--bg-card);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 48px 44px;
}

.login-card { 
  width: 100%; 
  max-width: 380px;
  animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.login-header { margin-bottom: 32px; }
.login-header h2 {
  font-size: 26px;
  font-weight: 800;
  letter-spacing: -0.5px;
  margin-bottom: 6px;
}
.login-header p { font-size: 14px; color: var(--text-secondary); }

.login-form { display: flex; flex-direction: column; gap: 20px; }

.form-group { display: flex; flex-direction: column; gap: 6px; }
label { font-size: 13px; font-weight: 600; color: var(--text-secondary); }

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 14px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  backdrop-filter: blur(8px);
}
.input-wrapper:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.2);
}
.input-wrapper:focus-within {
  border-color: var(--primary);
  background: rgba(108, 99, 255, 0.08);
  box-shadow: 0 0 0 4px rgba(108, 99, 255, 0.15);
}
.form-group.error .input-wrapper { border-color: var(--danger); }

.input-icon {
  padding: 0 12px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  flex-shrink: 0;
}
input[type="email"], input[type="password"], input[type="text"] {
  flex: 1;
  background: transparent !important;
  border: none;
  outline: none;
  font-family: 'Inter', sans-serif;
  font-size: 15px;
  color: var(--text-primary);
  padding: 14px 16px 14px 0;
}

/* Fix browser autofill background */
input:-webkit-autofill,
input:-webkit-autofill:hover, 
input:-webkit-autofill:focus, 
input:-webkit-autofill:active {
  -webkit-box-shadow: 0 0 0 50px #1a1a2e inset !important; /* Matches var(--bg-card) */
  -webkit-text-fill-color: var(--text-primary) !important;
  transition: background-color 5000s ease-in-out 0s;
}

input::placeholder { color: var(--text-muted); opacity: 0.6; }

.toggle-password {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0 12px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  transition: color 0.2s;
}
.toggle-password:hover { color: var(--primary); }

.error-msg { font-size: 12px; color: var(--danger); padding-left: 2px; }

.form-options {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.checkbox-label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-secondary);
  cursor: pointer;
}
.checkbox-label input[type="checkbox"] { display: none; }
.checkbox-custom {
  width: 18px; height: 18px;
  border-radius: 6px;
  border: 1.5px solid rgba(255, 255, 255, 0.15);
  background: rgba(255, 255, 255, 0.05);
  transition: all 0.2s;
  display: flex; align-items: center; justify-content: center;
}
.checkbox-label input:checked + .checkbox-custom {
  background: var(--primary);
  border-color: var(--primary);
  box-shadow: 0 0 10px rgba(108, 99, 255, 0.4);
}
.forgot-link {
  font-size: 13px;
  color: var(--primary-light);
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
}
.forgot-link:hover { color: var(--primary); }

.login-error {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(239,68,68,0.1);
  border: 1px solid rgba(239,68,68,0.25);
  border-radius: 10px;
  padding: 12px 14px;
  font-size: 13px;
  color: #fca5a5;
}

.submit-btn {
  width: 100%;
  justify-content: center;
  padding: 14px;
  font-size: 15px;
}
.submit-btn:disabled { opacity: 0.7; cursor: not-allowed; transform: none !important; }

.spinner {
  animation: spin 0.8s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }

.demo-hint {
  text-align: center;
  font-size: 12px;
  color: var(--text-muted);
  background: rgba(255,255,255,0.03);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 10px;
}
.demo-hint code {
  background: rgba(108,99,255,0.2);
  color: var(--primary-light);
  padding: 1px 6px;
  border-radius: 4px;
  font-size: 12px;
}

@media (max-width: 768px) {
  .login-left { display: none; }
  .login-right { padding: 40px 28px; }
}
</style>
