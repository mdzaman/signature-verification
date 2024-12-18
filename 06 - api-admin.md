# API Admin Portal

```tsx
import React, { useState } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Alert, AlertDescription } from '@/components/ui/alert';
import { 
  Settings, Key, Shield, Database, 
  Globe, Copy, CheckCircle, RefreshCw,
  Code, Lock, Zap, Users
} from 'lucide-react';

const AdminPortal = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [apiKey, setApiKey] = useState('sk_live_xxxx');
  const [showKey, setShowKey] = useState(false);
  const [copied, setCopied] = useState(false);

  const copyKey = () => {
    navigator.clipboard.writeText(apiKey);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  };

  const tabs = [
    { id: 'dashboard', icon: Settings, label: 'Dashboard' },
    { id: 'keys', icon: Key, label: 'API Keys' },
    { id: 'security', icon: Shield, label: 'Security' },
    { id: 'usage', icon: Database, label: 'Usage & Billing' },
  ];

  const quickStats = [
    { id: 'requests', icon: Globe, label: 'API Requests', value: '1.2M', trend: '+12%' },
    { id: 'latency', icon: Zap, label: 'Avg. Latency', value: '120ms', trend: '-5%' },
    { id: 'users', icon: Users, label: 'Active Users', value: '850', trend: '+8%' },
    { id: 'success', icon: CheckCircle, label: 'Success Rate', value: '99.9%', trend: '+0.1%' },
  ];

  const curlExample = `curl -X POST https://api.yourdomain.com/v1/verify \\
  -H "Authorization: Bearer ${apiKey}" \\
  -H "Content-Type: application/json" \\
  -d '{"signature": "base64_encoded_image"}'`;

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="max-w-7xl mx-auto space-y-6">
        {/* Header */}
        <div className="flex justify-between items-center">
          <h1 className="text-3xl font-bold text-gray-900">API Administration</h1>
          <Button variant="outline">
            <RefreshCw className="mr-2 h-4 w-4" />
            Refresh Data
          </Button>
        </div>

        {/* Quick Stats */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
          {quickStats.map((stat) => (
            <Card key={stat.id}>
              <CardContent className="p-6">
                <div className="flex items-center justify-between">
                  <div className="flex items-center">
                    <stat.icon className="h-8 w-8 text-blue-500" />
                    <div className="ml-4">
                      <p className="text-sm text-gray-500">{stat.label}</p>
                      <p className="text-2xl font-bold">{stat.value}</p>
                    </div>
                  </div>
                  <span className={`text-sm ${
                    stat.trend.startsWith('+') ? 'text-green-500' : 'text-red-500'
                  }`}>
                    {stat.trend}
                  </span>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>

        {/* Main Content */}
        <div className="grid grid-cols-12 gap-6">
          {/* Sidebar */}
          <Card className="col-span-12 md:col-span-3">
            <CardContent className="p-4">
              <nav className="space-y-2">
                {tabs.map((tab) => (
                  <Button
                    key={tab.id}
                    variant={activeTab === tab.id ? "default" : "ghost"}
                    className="w-full justify-start"
                    onClick={() => setActiveTab(tab.id)}
                  >
                    <tab.icon className="mr-2 h-4 w-4" />
                    {tab.label}
                  </Button>
                ))}
              </nav>
            </CardContent>
          </Card>

          {/* Main Panel */}
          <div className="col-span-12 md:col-span-9 space-y-6">
            {/* API Keys Section */}
            <Card>
              <CardHeader>
                <CardTitle className="text-xl">API Configuration</CardTitle>
              </CardHeader>
              <CardContent className="space-y-6">
                <div className="space-y-4">
                  <div className="flex items-center justify-between">
                    <div className="space-y-1">
                      <h3 className="text-lg font-medium">Live API Key</h3>
                      <p className="text-sm text-gray-500">Use this key for production requests</p>
                    </div>
                    <Button
                      variant="outline"
                      onClick={copyKey}
                      className="flex items-center"
                    >
                      {copied ? <CheckCircle className="h-4 w-4" /> : <Copy className="h-4 w-4" />}
                      <span className="ml-2">{copied ? 'Copied!' : 'Copy'}</span>
                    </Button>
                  </div>
                  
                  <div className="flex items-center space-x-2">
                    <code className="flex-1 p-3 bg-gray-100 rounded text-sm font-mono">
                      {showKey ? apiKey : '•'.repeat(apiKey.length)}
                    </code>
                    <Button
                      variant="ghost"
                      onClick={() => setShowKey(!showKey)}
                      size="sm"
                    >
                      {showKey ? 'Hide' : 'Show'}
                    </Button>
                  </div>
                </div>

                {/* Configuration Options */}
                <div className="space-y-4">
                  <h3 className="text-lg font-medium">API Settings</h3>
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <Card>
                      <CardContent className="p-4">
                        <div className="flex items-center justify-between">
                          <div className="flex items-center">
                            <Lock className="h-5 w-5 text-blue-500" />
                            <span className="ml-2">Rate Limiting</span>
                          </div>
                          <Button variant="outline" size="sm">Configure</Button>
                        </div>
                      </CardContent>
                    </Card>
                    
                    <Card>
                      <CardContent className="p-4">
                        <div className="flex items-center justify-between">
                          <div className="flex items-center">
                            <Code className="h-5 w-5 text-blue-500" />
                            <span className="ml-2">Webhooks</span>
                          </div>
                          <Button variant="outline" size="sm">Configure</Button>
                        </div>
                      </CardContent>
                    </Card>
                  </div>
                </div>

                {/* Documentation */}
                <div className="bg-blue-50 p-4 rounded-lg">
                  <h3 className="text-lg font-medium text-blue-700">Quick Start</h3>
                  <div className="mt-2 space-y-2">
                    <pre className="block p-3 bg-white rounded text-sm font-mono overflow-x-auto">
                      {curlExample}
                    </pre>
                  </div>
                </div>
              </CardContent>
            </Card>
          </div>
        </div>
      </div>
    </div>
  );
};

export default AdminPortal;

```

The main fixes include:
1. Moved the curl command example to a template literal variable
2. Properly escaped the API key interpolation
3. Used a `pre` tag instead of `code` for multi-line code blocks
4. Fixed JSX children nesting and formatting
5. Ensured all JSX expressions are properly closed

The functionality remains the same, but the component should now render without any syntax errors. Would you like me to make any additional improvements to the interface or add more features?
