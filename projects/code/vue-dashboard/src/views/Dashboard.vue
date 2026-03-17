<template>
  <div class="dashboard-layout">
    <!-- Sidebar -->
    <aside class="sidebar" :class="{ collapsed: sidebarCollapsed }">
      <div class="sidebar-header">
        <div class="brand">
          <div class="brand-icon">
            <img src="https://sysinfra.in/img/mobile-logo.png" alt="Logo" width="28" height="28" style="object-fit: contain;" />
          </div>
          <span v-if="!sidebarCollapsed" class="brand-name">System Infra Solutions</span>
        </div>
        <button class="collapse-btn" @click="sidebarCollapsed = !sidebarCollapsed">
          <svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <div class="nav-section-label" v-if="!sidebarCollapsed">MAIN</div>
        <a v-for="item in navItems" :key="item.label"
           class="nav-item" :class="{ active: activeNav === item.label }"
           @click="activeNav = item.label" href="#">
          <span class="nav-icon" v-html="item.icon"></span>
          <span v-if="!sidebarCollapsed" class="nav-label">{{ item.label }}</span>
          <span v-if="!sidebarCollapsed && item.badge" class="nav-badge">{{ item.badge }}</span>
        </a>

        <div class="nav-section-label" style="margin-top:16px;" v-if="!sidebarCollapsed">SETTINGS</div>
        <a v-for="item in navSettings" :key="item.label"
           class="nav-item" :class="{ active: activeNav === item.label }"
           @click="activeNav = item.label" href="#">
          <span class="nav-icon" v-html="item.icon"></span>
          <span v-if="!sidebarCollapsed" class="nav-label">{{ item.label }}</span>
        </a>
      </nav>

      <div class="sidebar-profile" v-if="!sidebarCollapsed">
        <div class="profile-avatar">R</div>
        <div class="profile-info">
          <div class="profile-name">{{ userName }}</div>
          <div class="profile-role">Administrator</div>
        </div>
        <button class="logout-btn" @click="handleLogout" title="Logout">
          <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/>
          </svg>
        </button>
      </div>
    </aside>

    <!-- Main Content -->
    <div class="main-content">
      <!-- Top Navbar -->
      <header class="topbar">
        <div class="topbar-left">
          <h1 class="page-title">{{ activeNav }}</h1>
          <div class="breadcrumb">
            <span>Home</span>
            <span class="sep">›</span>
            <span class="current">{{ activeNav }}</span>
          </div>
        </div>
        <div class="topbar-right">
          <div class="search-box">
            <svg width="15" height="15" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
            </svg>
            <input placeholder="Search..." />
          </div>
          <button class="icon-btn">
            <svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/>
            </svg>
            <span class="icon-btn-badge">3</span>
          </button>
          <button class="icon-btn">
            <svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z"/>
            </svg>
          </button>
          <div class="user-chip">
            <div class="user-avatar">R</div>
            <span>{{ userName.split(' ')[0] }}</span>
          </div>
        </div>
      </header>

      <!-- Dashboard Body -->
      <main class="dashboard-body">
        <!-- Greeting -->
        <div class="greeting-section">
          <div>
            <h2>Good {{ greeting }}, {{ userName.split(' ')[0] }}! 👋</h2>
            <p>Here's what's happening with your business today.</p>
          </div>
          <button class="btn btn-primary">
            <svg width="16" height="16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>
            </svg>
            New Report
          </button>
        </div>

        <!-- Stats Cards -->
        <div class="stats-grid">
          <div v-for="stat in stats" :key="stat.label" class="stat-card">
            <div class="stat-top">
              <div class="stat-icon" :style="{ background: stat.iconBg }">
                <span v-html="stat.icon"></span>
              </div>
              <span class="stat-change" :class="stat.positive ? 'up' : 'down'">
                {{ stat.positive ? '▲' : '▼' }} {{ stat.change }}
              </span>
            </div>
            <div class="stat-value">{{ stat.value }}</div>
            <div class="stat-label">{{ stat.label }}</div>
            <div class="stat-bar">
              <div class="stat-bar-fill" :style="{ width: stat.progress + '%', background: stat.color }"></div>
            </div>
          </div>
        </div>

        <!-- Charts Row -->
        <div class="charts-row">
          <!-- Activity Chart -->
          <div class="card chart-card">
            <div class="card-header">
              <div>
                <div class="card-title">Revenue Overview</div>
                <div class="card-subtitle">Monthly performance</div>
              </div>
              <div class="chart-tabs">
                <button v-for="t in ['Week','Month','Year']" :key="t"
                  class="chart-tab" :class="{ active: activeTab === t }"
                  @click="activeTab = t">{{ t }}</button>
              </div>
            </div>
            <div class="bar-chart">
              <div v-for="(bar, i) in chartData" :key="i" class="bar-col">
                <div class="bar-wrap">
                  <div class="bar" :style="{ height: bar.height + '%', background: bar.active ? 'linear-gradient(180deg, #6C63FF, #4f46e5)' : 'rgba(108,99,255,0.25)' }">
                    <span class="bar-tip" v-if="bar.active">{{ bar.value }}</span>
                  </div>
                </div>
                <span class="bar-label">{{ bar.label }}</span>
              </div>
            </div>
          </div>

          <!-- Donut / Summary -->
          <div class="card summary-card">
            <div class="card-header">
              <div>
                <div class="card-title">Traffic Sources</div>
                <div class="card-subtitle">This month</div>
              </div>
            </div>
            <div class="donut-container">
              <svg viewBox="0 0 120 120" class="donut-svg">
                <circle cx="60" cy="60" r="48" fill="none" stroke="#1a1a2e" stroke-width="20"/>
                <circle cx="60" cy="60" r="48" fill="none" stroke="#6C63FF" stroke-width="20"
                  stroke-dasharray="180 121" stroke-dashoffset="0" stroke-linecap="round"/>
                <circle cx="60" cy="60" r="48" fill="none" stroke="#06b6d4" stroke-width="20"
                  stroke-dasharray="75 225" stroke-dashoffset="-180" stroke-linecap="round"/>
                <circle cx="60" cy="60" r="48" fill="none" stroke="#10b981" stroke-width="20"
                  stroke-dasharray="46 253" stroke-dashoffset="-255" stroke-linecap="round"/>
              </svg>
              <div class="donut-center">
                <div class="donut-total">100%</div>
                <div class="donut-sub">Total</div>
              </div>
            </div>
            <div class="legend">
              <div v-for="s in sources" :key="s.name" class="legend-item">
                <span class="legend-dot" :style="{ background: s.color }"></span>
                <span class="legend-name">{{ s.name }}</span>
                <span class="legend-pct">{{ s.pct }}%</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Recent Activity & Top Users Row -->
        <div class="bottom-row">
          <!-- Recent Transactions -->
          <div class="card transactions-card">
            <div class="card-header">
              <div>
                <div class="card-title">Recent Transactions</div>
                <div class="card-subtitle">Last 24 hours</div>
              </div>
              <a href="#" class="view-all">View all →</a>
            </div>
            <div class="table-wrapper">
              <table class="data-table">
                <thead>
                  <tr>
                    <th>User</th>
                    <th>Amount</th>
                    <th>Status</th>
                    <th>Date</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="t in transactions" :key="t.id">
                    <td>
                      <div class="user-cell">
                        <div class="u-avatar" :style="{ background: t.color }">{{ t.initials }}</div>
                        <div>
                          <div class="u-name">{{ t.name }}</div>
                          <div class="u-email">{{ t.email }}</div>
                        </div>
                      </div>
                    </td>
                    <td class="amount-cell">{{ t.amount }}</td>
                    <td><span class="status-badge" :class="t.status">{{ t.status }}</span></td>
                    <td class="date-cell">{{ t.date }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Quick Actions -->
          <div class="card quick-card">
            <div class="card-header">
              <div>
                <div class="card-title">Quick Actions</div>
                <div class="card-subtitle">Shortcuts</div>
              </div>
            </div>
            <div class="quick-actions">
              <button v-for="action in quickActions" :key="action.label" class="quick-btn">
                <div class="quick-icon" :style="{ background: action.bg }">
                  <span v-html="action.icon"></span>
                </div>
                <span>{{ action.label }}</span>
              </button>
            </div>
            <div class="goal-section">
              <div class="goal-header">
                <span>Monthly Goal</span>
                <span class="goal-pct">72%</span>
              </div>
              <div class="goal-bar"><div class="goal-fill"></div></div>
              <div class="goal-sub">$36,000 of $50,000 reached</div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const sidebarCollapsed = ref(false)
const activeNav = ref('Dashboard')
const activeTab = ref('Month')
const userName = localStorage.getItem('user_name') || 'Rajesh'

const hour = new Date().getHours()
const greeting = computed(() => hour < 12 ? 'morning' : hour < 17 ? 'afternoon' : 'evening')

function handleLogout() {
  localStorage.removeItem('auth_token')
  localStorage.removeItem('user_name')
  localStorage.removeItem('user_email')
  router.push('/login')
}

const navItems = [
  { label: 'Dashboard', badge: null, icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/></svg>' },
  { label: 'Analytics', badge: null, icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/></svg>' },
  { label: 'Users', badge: '24', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z"/></svg>' },
  { label: 'Projects', badge: '5', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"/></svg>' },
  { label: 'Reports', badge: null, icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>' },
]

const navSettings = [
  { label: 'Settings', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/></svg>' },
  { label: 'Help', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>' },
]

const stats = [
  { label: 'Total Revenue', value: '$84,254', change: '12.5%', positive: true, progress: 75, color: '#6C63FF', iconBg: 'rgba(108,99,255,0.15)', icon: '<svg width="22" height="22" fill="none" viewBox="0 0 24 24" stroke="#6C63FF"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>' },
  { label: 'Active Users', value: '12,483', change: '8.2%', positive: true, progress: 60, color: '#06b6d4', iconBg: 'rgba(6,182,212,0.15)', icon: '<svg width="22" height="22" fill="none" viewBox="0 0 24 24" stroke="#06b6d4"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z"/></svg>' },
  { label: 'Conversions', value: '3,842', change: '4.1%', positive: false, progress: 42, color: '#f59e0b', iconBg: 'rgba(245,158,11,0.15)', icon: '<svg width="22" height="22" fill="none" viewBox="0 0 24 24" stroke="#f59e0b"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/></svg>' },
  { label: 'Satisfaction', value: '98.3%', change: '2.4%', positive: true, progress: 98, color: '#10b981', iconBg: 'rgba(16,185,129,0.15)', icon: '<svg width="22" height="22" fill="none" viewBox="0 0 24 24" stroke="#10b981"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>' },
]

const chartData = [
  { label: 'Jan', height: 45, value: '$4.2k', active: false },
  { label: 'Feb', height: 60, value: '$5.8k', active: false },
  { label: 'Mar', height: 40, value: '$3.9k', active: false },
  { label: 'Apr', height: 75, value: '$7.1k', active: false },
  { label: 'May', height: 55, value: '$5.2k', active: false },
  { label: 'Jun', height: 85, value: '$8.4k', active: true },
  { label: 'Jul', height: 65, value: '$6.1k', active: false },
  { label: 'Aug', height: 90, value: '$9.2k', active: false },
  { label: 'Sep', height: 70, value: '$6.8k', active: false },
  { label: 'Oct', height: 80, value: '$7.9k', active: false },
  { label: 'Nov', height: 50, value: '$4.7k', active: false },
  { label: 'Dec', height: 95, value: '$9.8k', active: false },
]

const sources = [
  { name: 'Organic', pct: 60, color: '#6C63FF' },
  { name: 'Referral', pct: 25, color: '#06b6d4' },
  { name: 'Direct', pct: 15, color: '#10b981' },
]

const transactions = [
  { id:1, name:'Sarah Chen', email:'sarah@example.com', initials:'SC', color:'#6C63FF', amount:'+$1,240', status:'completed', date:'Mar 16' },
  { id:2, name:'Mark Rivera', email:'mark@example.com', initials:'MR', color:'#06b6d4', amount:'+$850', status:'pending', date:'Mar 16' },
  { id:3, name:'Emily Park', email:'emily@example.com', initials:'EP', color:'#10b981', amount:'-$220', status:'failed', date:'Mar 15' },
  { id:4, name:'James Liu', email:'james@example.com', initials:'JL', color:'#f59e0b', amount:'+$3,100', status:'completed', date:'Mar 15' },
  { id:5, name:'Ana Ruiz', email:'ana@example.com', initials:'AR', color:'#ef4444', amount:'+$760', status:'pending', date:'Mar 14' },
]

const quickActions = [
  { label: 'Add User', bg: 'rgba(108,99,255,0.15)', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="#6C63FF"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z"/></svg>' },
  { label: 'New Report', bg: 'rgba(6,182,212,0.15)', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="#06b6d4"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>' },
  { label: 'Schedule', bg: 'rgba(16,185,129,0.15)', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="#10b981"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/></svg>' },
  { label: 'Export', bg: 'rgba(245,158,11,0.15)', icon: '<svg width="18" height="18" fill="none" viewBox="0 0 24 24" stroke="#f59e0b"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>' },
]
</script>

<style scoped>
.dashboard-layout {
  display: flex;
  height: 100vh;
  overflow: hidden;
  background: var(--bg-dark);
}

/* ---- Sidebar ---- */
.sidebar {
  width: var(--sidebar-width);
  background: var(--bg-card);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  transition: width 0.3s ease;
  overflow: hidden;
  flex-shrink: 0;
  z-index: 10;
}
.sidebar.collapsed { width: 72px; }

.sidebar-header {
  padding: 20px 14px;
  display: flex;
  align-items: center;
  gap: 8px;
  justify-content: space-between;
  border-bottom: 1px solid var(--border);
}
.brand { display: flex; align-items: center; gap: 8px; min-width: 0; flex: 1; }
.brand-name { font-size: 16px; font-weight: 800; color: var(--text-primary); white-space: nowrap; letter-spacing: -0.3px; overflow: hidden; text-overflow: ellipsis; }
.collapse-btn {
  background: rgba(255,255,255,0.06);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 6px;
  cursor: pointer;
  color: var(--text-secondary);
  display: flex;
  flex-shrink: 0;
  transition: all 0.2s;
}
.collapse-btn:hover { background: rgba(108,99,255,0.2); color: var(--primary-light); }

.sidebar-nav { flex: 1; padding: 16px 10px; overflow-y: auto; display: flex; flex-direction: column; gap: 2px; }
.nav-section-label {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 1.2px;
  color: var(--text-muted);
  padding: 0 8px;
  margin-bottom: 4px;
  margin-top: 4px;
  white-space: nowrap;
}
.nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border-radius: 10px;
  cursor: pointer;
  text-decoration: none;
  color: var(--text-secondary);
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s;
  white-space: nowrap;
  position: relative;
}
.nav-item:hover { background: rgba(108,99,255,0.1); color: var(--text-primary); }
.nav-item.active { background: rgba(108,99,255,0.18); color: var(--primary-light); }
.nav-item.active .nav-icon { color: var(--primary); }
.nav-icon { flex-shrink: 0; display: flex; }
.nav-label { flex: 1; }
.nav-badge {
  background: var(--primary);
  color: #fff;
  font-size: 11px;
  font-weight: 700;
  padding: 1px 7px;
  border-radius: 20px;
}

.sidebar-profile {
  padding: 16px;
  border-top: 1px solid var(--border);
  display: flex;
  align-items: center;
  gap: 10px;
}
.profile-avatar {
  width: 36px; height: 36px;
  border-radius: 10px;
  background: linear-gradient(135deg, #6C63FF, #06b6d4);
  color: #fff;
  font-size: 13px;
  font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.profile-info { flex: 1; min-width: 0; }
.profile-name { font-size: 13px; font-weight: 600; color: var(--text-primary); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.profile-role { font-size: 11px; color: var(--text-muted); }
.logout-btn {
  background: none;
  border: none;
  cursor: pointer;
  color: var(--text-secondary);
  display: flex;
  padding: 6px;
  border-radius: 8px;
  transition: all 0.2s;
  flex-shrink: 0;
}
.logout-btn:hover { color: var(--danger); background: rgba(239,68,68,0.1); }

/* ---- Main Content ---- */
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Topbar */
.topbar {
  height: 64px;
  background: var(--bg-card);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 28px;
  flex-shrink: 0;
}
.page-title { font-size: 18px; font-weight: 700; color: var(--text-primary); }
.breadcrumb { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
.breadcrumb .sep { margin: 0 6px; }
.breadcrumb .current { color: var(--primary-light); }

.topbar-right { display: flex; align-items: center; gap: 12px; }
.search-box {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.05);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 8px 14px;
  color: var(--text-muted);
}
.search-box input {
  background: none;
  border: none;
  outline: none;
  font-family: 'Inter', sans-serif;
  font-size: 13px;
  color: var(--text-primary);
  width: 160px;
}
.search-box input::placeholder { color: var(--text-muted); }

.icon-btn {
  background: rgba(255,255,255,0.05);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 9px;
  cursor: pointer;
  color: var(--text-secondary);
  display: flex;
  position: relative;
  transition: all 0.2s;
}
.icon-btn:hover { background: rgba(108,99,255,0.15); color: var(--primary-light); }
.icon-btn-badge {
  position: absolute;
  top: -4px; right: -4px;
  background: var(--danger);
  color: #fff;
  font-size: 9px;
  font-weight: 700;
  width: 15px; height: 15px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
}
.user-chip {
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.05);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 6px 12px 6px 6px;
  cursor: pointer;
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
}
.user-avatar {
  width: 28px; height: 28px;
  border-radius: 8px;
  background: linear-gradient(135deg, #6C63FF, #06b6d4);
  color: #fff;
  font-size: 11px;
  font-weight: 700;
  display: flex; align-items: center; justify-content: center;
}

/* Dashboard body */
.dashboard-body { flex:1; overflow-y:auto; padding: 28px; display: flex; flex-direction: column; gap: 24px; }

.greeting-section {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.greeting-section h2 { font-size: 22px; font-weight: 800; letter-spacing: -0.3px; }
.greeting-section p { font-size: 14px; color: var(--text-secondary); margin-top: 4px; }

/* Stats */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 18px;
}
.stat-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 20px;
  transition: transform 0.2s, box-shadow 0.2s;
}
.stat-card:hover { transform: translateY(-3px); box-shadow: 0 12px 30px rgba(0,0,0,0.3); }
.stat-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.stat-icon {
  width: 44px; height: 44px;
  border-radius: 12px;
  display: flex; align-items: center; justify-content: center;
}
.stat-change { font-size: 12px; font-weight: 600; padding: 3px 8px; border-radius: 20px; }
.stat-change.up { color: #34d399; background: rgba(52,211,153,0.12); }
.stat-change.down { color: #f87171; background: rgba(248,113,113,0.12); }
.stat-value { font-size: 26px; font-weight: 800; letter-spacing: -0.5px; margin-bottom: 4px; }
.stat-label { font-size: 13px; color: var(--text-secondary); margin-bottom: 14px; }
.stat-bar { height: 4px; background: rgba(255,255,255,0.06); border-radius: 4px; overflow: hidden; }
.stat-bar-fill { height: 100%; border-radius: 4px; transition: width 0.8s ease; }

/* Cards */
.card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 24px;
  display: flex;
  flex-direction: column;
  height: 100%;
}
.card-header { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 20px; }
.card-title { font-size: 15px; font-weight: 700; color: var(--text-primary); }
.card-subtitle { font-size: 12px; color: var(--text-muted); margin-top: 3px; }
.view-all { font-size: 13px; color: var(--primary-light); text-decoration: none; font-weight: 500; }
.view-all:hover { color: var(--primary); }

/* Charts Row */
.charts-row { display: grid; grid-template-columns: 1.6fr 1fr; gap: 20px; align-items: stretch; }

.chart-tabs { display: flex; gap: 4px; }
.chart-tab {
  background: none;
  border: none;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 600;
  color: var(--text-muted);
  padding: 5px 10px;
  border-radius: 8px;
  transition: all 0.2s;
}
.chart-tab.active { background: rgba(108,99,255,0.2); color: var(--primary-light); }

.bar-chart { display: flex; align-items: flex-end; gap: 8px; height: 160px; }
.bar-col { display: flex; flex-direction: column; align-items: center; gap: 6px; flex: 1; height: 100%; }
.bar-wrap { flex: 1; display: flex; align-items: flex-end; width: 100%; }
.bar {
  width: 100%;
  border-radius: 6px 6px 0 0;
  position: relative;
  transition: height 0.5s ease;
  min-height: 6px;
}
.bar-tip {
  position: absolute;
  top: -24px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 11px;
  font-weight: 700;
  color: var(--primary-light);
  white-space: nowrap;
}
.bar-label { font-size: 11px; color: var(--text-muted); }

/* Donut */
.donut-container { position: relative; display: flex; align-items: center; justify-content: center; margin: 10px 0; }
.donut-svg { width: 160px; height: 160px; transform: rotate(-90deg); }
.donut-center { position: absolute; text-align: center; }
.donut-total { font-size: 24px; font-weight: 800; }
.donut-sub { font-size: 12px; color: var(--text-muted); }
.legend { display: flex; flex-direction: column; gap: 10px; }
.legend-item { display: flex; align-items: center; gap: 8px; font-size: 13px; }
.legend-dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }
.legend-name { flex: 1; color: var(--text-secondary); }
.legend-pct { font-weight: 700; color: var(--text-primary); }

/* Bottom Row */
.bottom-row { display: grid; grid-template-columns: 1.8fr 1fr; gap: 20px; align-items: stretch; }

.table-wrapper { overflow-x: auto; flex: 1; }
.data-table { width: 100%; border-collapse: collapse; }
.data-table th {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.8px;
  color: var(--text-muted);
  text-align: left;
  padding: 8px 12px;
  border-bottom: 1px solid var(--border);
}
.data-table td { padding: 12px 12px; border-bottom: 1px solid rgba(255,255,255,0.04); font-size: 13px; }
.data-table tr:last-child td { border-bottom: none; }
.data-table tr:hover td { background: rgba(255,255,255,0.02); }

.user-cell { display: flex; align-items: center; gap: 10px; }
.u-avatar {
  width: 34px; height: 34px;
  border-radius: 10px;
  font-size: 12px;
  font-weight: 700;
  color: #fff;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.u-name { font-size: 13px; font-weight: 600; color: var(--text-primary); }
.u-email { font-size: 11px; color: var(--text-muted); }
.amount-cell { font-weight: 700; color: var(--text-primary); }
.date-cell { color: var(--text-muted); font-size: 12px; }

.status-badge {
  font-size: 11px;
  font-weight: 700;
  padding: 3px 10px;
  border-radius: 20px;
  text-transform: capitalize;
}
.status-badge.completed { background: rgba(16,185,129,0.15); color: #34d399; }
.status-badge.pending { background: rgba(245,158,11,0.15); color: #fbbf24; }
.status-badge.failed { background: rgba(239,68,68,0.15); color: #f87171; }

/* Quick Actions */
.quick-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: auto; }
.quick-btn {
  background: rgba(255,255,255,0.03);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 14px 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 600;
  color: var(--text-secondary);
  transition: all 0.2s;
}
.quick-btn:hover { border-color: rgba(108,99,255,0.4); background: rgba(108,99,255,0.08); color: var(--text-primary); transform: translateY(-2px); }
.quick-icon { width: 36px; height: 36px; border-radius: 10px; display: flex; align-items: center; justify-content: center; }

.goal-section { border-top: 1px solid var(--border); padding-top: 16px; }
.goal-header { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 8px; }
.goal-header span:first-child { color: var(--text-secondary); font-weight: 500; }
.goal-pct { color: var(--primary-light); font-weight: 700; }
.goal-bar { height: 6px; background: rgba(255,255,255,0.06); border-radius: 6px; margin-bottom: 8px; }
.goal-fill { width: 72%; height: 100%; border-radius: 6px; background: linear-gradient(90deg, var(--primary), var(--accent)); }
.goal-sub { font-size: 12px; color: var(--text-muted); }
</style>
