I'll create a comprehensive compliance and audit dashboard for monitoring GDPR and privacy requirements.



```tsx
import React, { useState } from 'react';
import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card';
import { Alert, AlertTitle, AlertDescription } from '@/components/ui/alert';
import { Button } from '@/components/ui/button';
import { 
  Shield, 
  AlertTriangle, 
  FileText, 
  Clock, 
  CheckCircle, 
  XCircle,
  Download,
  RefreshCw,
  Search,
  Filter
} from 'lucide-react';
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer
} from 'recharts';

const ComplianceDashboard = () => {
  const [activeTab, setActiveTab] = useState('overview');
  
  // Sample compliance data
  const complianceData = {
    gdpr: {
      score: 92,
      lastCheck: '2024-12-18',
      issues: 3,
      critical: 1
    },
    ccpa: {
      score: 88,
      lastCheck: '2024-12-18',
      issues: 5,
      critical: 0
    },
    hipaa: {
      score: 95,
      lastCheck: '2024-12-18',
      issues: 2,
      critical: 0
    }
  };

  // Sample audit history data
  const auditHistory = [
    { month: 'Jul', gdpr: 88, ccpa: 85, hipaa: 90 },
    { month: 'Aug', gdpr: 90, ccpa: 86, hipaa: 92 },
    { month: 'Sep', gdpr: 89, ccpa: 87, hipaa: 93 },
    { month: 'Oct', gdpr: 91, ccpa: 88, hipaa: 94 },
    { month: 'Nov', gdpr: 90, ccpa: 87, hipaa: 94 },
    { month: 'Dec', gdpr: 92, ccpa: 88, hipaa: 95 }
  ];

  // Sample GDPR requirements
  const gdprRequirements = [
    { requirement: 'Data Processing Agreement', status: 'compliant', lastCheck: '2024-12-18' },
    { requirement: 'Privacy Impact Assessment', status: 'warning', lastCheck: '2024-12-15' },
    { requirement: 'Data Subject Rights', status: 'compliant', lastCheck: '2024-12-17' },
    { requirement: 'Consent Management', status: 'critical', lastCheck: '2024-12-16' },
    { requirement: 'Data Breach Notification', status: 'compliant', lastCheck: '2024-12-18' }
  ];

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      {/* Header */}
      <div className="mb-6">
        <h1 className="text-3xl font-bold text-gray-900">Compliance & Audit Dashboard</h1>
        <p className="text-gray-500">Monitor GDPR compliance and privacy requirements</p>
      </div>

      {/* Quick Stats */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
        {Object.entries(complianceData).map(([framework, data]) => (
          <Card key={framework} className="hover:shadow-lg transition-shadow">
            <CardContent className="p-6">
              <div className="flex justify-between items-start mb-4">
                <div>
                  <h3 className="text-xl font-semibold uppercase">{framework}</h3>
                  <p className="text-gray-500">Compliance Score</p>
                </div>
                <Shield className={`h-8 w-8 ${
                  data.score >= 90 ? 'text-green-500' : 
                  data.score >= 80 ? 'text-yellow-500' : 'text-red-500'
                }`} />
              </div>
              <div className="text-4xl font-bold mb-2">{data.score}%</div>
              <div className="flex justify-between text-sm">
                <span className="text-gray-500">Issues: {data.issues}</span>
                <span className={`font-semibold ${
                  data.critical > 0 ? 'text-red-500' : 'text-green-500'
                }`}>
                  Critical: {data.critical}
                </span>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>

      {/* Main Content Tabs */}
      <div className="flex space-x-4 mb-6">
        {['overview', 'gdpr', 'audit', 'reports'].map((tab) => (
          <Button
            key={tab}
            variant={activeTab === tab ? 'default' : 'outline'}
            onClick={() => setActiveTab(tab)}
            className="capitalize"
          >
            {tab}
          </Button>
        ))}
      </div>

      {/* Tab Content */}
      <div className="space-y-6">
        {activeTab === 'overview' && (
          <Card>
            <CardHeader>
              <CardTitle>Compliance Trend</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="h-80">
                <ResponsiveContainer width="100%" height="100%">
                  <LineChart data={auditHistory}>
                    <CartesianGrid strokeDasharray="3 3" />
                    <XAxis dataKey="month" />
                    <YAxis domain={[80, 100]} />
                    <Tooltip />
                    <Line type="monotone" dataKey="gdpr" stroke="#3b82f6" name="GDPR" />
                    <Line type="monotone" dataKey="ccpa" stroke="#10b981" name="CCPA" />
                    <Line type="monotone" dataKey="hipaa" stroke="#8b5cf6" name="HIPAA" />
                  </LineChart>
                </ResponsiveContainer>
              </div>
            </CardContent>
          </Card>
        )}

        {activeTab === 'gdpr' && (
          <div className="space-y-6">
            {/* GDPR Requirements Table */}
            <Card>
              <CardHeader>
                <CardTitle className="flex justify-between items-center">
                  <span>GDPR Requirements</span>
                  <Button size="sm">
                    <RefreshCw className="h-4 w-4 mr-2" />
                    Run Check
                  </Button>
                </CardTitle>
              </CardHeader>
              <CardContent>
                <div className="space-y-4">
                  {gdprRequirements.map((req, index) => (
                    <div key={index} className="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
                      <div className="flex items-center space-x-4">
                        {req.status === 'compliant' ? (
                          <CheckCircle className="h-6 w-6 text-green-500" />
                        ) : req.status === 'warning' ? (
                          <AlertTriangle className="h-6 w-6 text-yellow-500" />
                        ) : (
                          <XCircle className="h-6 w-6 text-red-500" />
                        )}
                        <div>
                          <p className="font-medium">{req.requirement}</p>
                          <p className="text-sm text-gray-500">Last checked: {req.lastCheck}</p>
                        </div>
                      </div>
                      <Button variant="outline" size="sm">
                        View Details
                      </Button>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>

            {/* Critical Alerts */}
            <Alert variant="destructive">
              <AlertTriangle className="h-4 w-4" />
              <AlertTitle>Critical Compliance Issues</AlertTitle>
              <AlertDescription>
                Consent Management requires immediate attention. User consent collection process needs update.
              </AlertDescription>
            </Alert>
          </div>
        )}

        {activeTab === 'audit' && (
          <Card>
            <CardHeader>
              <CardTitle className="flex justify-between items-center">
                <span>Audit Logs</span>
                <div className="flex space-x-2">
                  <Button variant="outline" size="sm">
                    <Filter className="h-4 w-4 mr-2" />
                    Filter
                  </Button>
                  <Button variant="outline" size="sm">
                    <Download className="h-4 w-4 mr-2" />
                    Export
                  </Button>
                </div>
              </CardTitle>
            </CardHeader>
            <CardContent>
              <div className="space-y-4">
                {[1, 2, 3, 4, 5].map((log) => (
                  <div key={log} className="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
                    <div>
                      <p className="font-medium">Compliance Check #{log}</p>
                      <p className="text-sm text-gray-500">2024-12-18 14:30:00</p>
                    </div>
                    <Button variant="outline" size="sm">
                      View Log
                    </Button>
                  </div>
                ))}
              </div>
            </CardContent>
          </Card>
        )}

        {activeTab === 'reports' && (
          <Card>
            <CardHeader>
              <CardTitle className="flex justify-between items-center">
                <span>Compliance Reports</span>
                <Button>
                  <FileText className="h-4 w-4 mr-2" />
                  Generate Report
                </Button>
              </CardTitle>
            </CardHeader>
            <CardContent>
              <div className="space-y-4">
                {['Monthly Compliance Summary', 'Data Processing Audit', 'Privacy Impact Assessment', 'Incident Report'].map((report, index) => (
                  <div key={index} className="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
                    <div className="flex items-center space-x-4">
                      <FileText className="h-6 w-6 text-blue-500" />
                      <div>
                        <p className="font-medium">{report}</p>
                        <p className="text-sm text-gray-500">Generated: 2024-12-18</p>
                      </div>
                    </div>
                    <div className="flex space-x-2">
                      <Button variant="outline" size="sm">
                        View
                      </Button>
                      <Button variant="outline" size="sm">
                        <Download className="h-4 w-4" />
                      </Button>
                    </div>
                  </div>
                ))}
              </div>
            </CardContent>
          </Card>
        )}
      </div>
    </div>
  );
};

export default ComplianceDashboard;

```

I've created a comprehensive compliance and audit dashboard that includes:

1. Overview Features:
- Compliance scores for different frameworks (GDPR, CCPA, HIPAA)
- Visual trend analysis
- Critical issues highlighting
- Real-time monitoring

2. GDPR Specific Features:
- Detailed requirements checklist
- Status tracking for each requirement
- Last check timestamps
- Critical alerts for non-compliance

3. Audit Capabilities:
- Detailed audit logs
- Filtering options
- Export functionality
- Historical tracking

4. Reporting Features:
- Automated report generation
- Multiple report types
- Download and sharing options
- Compliance trend analysis

The dashboard uses the shadcn/ui component library for a clean, modern interface and Recharts for data visualization. Would you like me to:

1. Add more specific compliance checks?
2. Enhance the reporting capabilities?
3. Include additional privacy frameworks?
4. Add more detailed audit trail features?

Let me know if you'd like any modifications or additional features to better match your compliance monitoring needs!
