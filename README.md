# Enabling-Exchange-Server-2019-Traces
Quick sample about how to dump ETL traces on Exchange Server 2019

- Exchange Traces Analysis is now in the form of a PowerShell script, leveraging Logman to create traces:

[Extra PowerShell Script](https://microsoft.github.io/CSS-Exchange/Diagnostics/ExTRA/)

- We must provide a config file to the script, named ```EnabledTraces.config``` ... here is a sample content to dump traces on a transport issue:

```
TraceLevels:Debug,Warning,Error,Fatal,Info,Performance,Function,Pfd
ActiveMonitoring:MailboxTransport,Transport
MessagingPolicies:TransportRulesEngine
StoreDriver:ModeratedTransport
StoreDriverDelivery:ModeratedTransport
StoreDriverSubmission:MailboxTransportSubmissionService,ModeratedTransport
Transport:AcceptedDomainsCache,AnonymousCertificateValidationResultCache,Approval,Certificate,Configuration,ContentConversion,DecisionEngine,DeliveryAgents,DeliveryProcessor,DSN,Dumpster,Expo,Extensibility,FaultInjection,General,HostedEncryption,HttpIn,HttpOut,HttpReceive,HttpSend,JournalingRulesCache,MessageResubmission,MessageTracking,MicrosoftExchangeRecipientCache,Orar,OrganizationSettingsCache,OutboundConnectorsCache,PerimeterSettingsCache,Pickup,Poison,PreviousHopLatency,ProxyHubSelector,Queuing,RemoteDomainsCache,Replication,Resolver,ResourceManager,ResourcePool,RightsManagement,Routing,Scheduler,SecureMail,Service,ShadowRedundancy,SmtpReceive,SmtpSend,Storage,Supervision,SyncDelivery,ThrottledForkAgent,TransportDumpster,TransportRulesCache,TransportSettingsCache,YammerAgent
FilteredTracing:No
InMemoryTracing:No
```

- Then we delete potential previous trace collection file:

```
logman delete ExchangeDebugTraces
```

- then after identifying the process we want to dump traces about, we can launch the trace with the information we want:

-- we first create the trace collection file

```
logman create trace ExchangeDebugTraces -p "{79bb49e6-2a2c-46e4-9167-fa122525d540}" -o c:\tracing\trace.etl -ow -f bin -max 1024 -mode globalsequence
```

-- then we start the trace itself

```
logman start ExchangeDebugTraces
```

-- we wait the issue to reproduce, and we stop the trace

```
logman stop ExchangeDebugTraces
```
