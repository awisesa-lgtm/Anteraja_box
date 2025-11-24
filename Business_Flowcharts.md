# Anteraja Campus HUB - Business Flowcharts

## 📊 Complete Business Flow Diagrams

---

## 1. Overall User Journey Flow

```mermaid
flowchart TD
    Start([User Needs to Send Package]) --> Channel{Choose Channel}
    
    Channel -->|Android App| AppLogin[Login Required]
    Channel -->|Web Platform| WebChoice{Guest or Register?}
    
    AppLogin --> AppDashboard[Dashboard]
    AppDashboard --> CreateOrder[Create Order]
    
    WebChoice -->|Guest| GuestOrder[Create Order - Guest]
    WebChoice -->|Register| WebLogin[Login/Register]
    WebLogin --> WebDashboard[Dashboard]
    WebDashboard --> CreateOrder
    
    GuestOrder --> GuestPayment[Payment]
    CreateOrder --> Payment[Payment]
    
    GuestPayment --> GuestQR[Generate QR Code]
    Payment --> QR[Generate QR Code]
    
    GuestQR --> GuestPrompt{Show Registration<br/>Incentive}
    GuestPrompt -->|Register| WebLogin
    GuestPrompt -->|Skip| DropPackage
    
    QR --> DropPackage[Go to Smart Box]
    
    DropPackage --> ScanQR[Scan QR at Smart Box]
    ScanQR --> InputQty[Input Package Quantity]
    InputQty --> PrintAWB[Print AWB]
    PrintAWB --> OpenDoor[Compartment Opens]
    OpenDoor --> InsertPackage[Insert Packages]
    InsertPackage --> CloseDoor[Close Door]
    
    CloseDoor --> Confirm{All Packages<br/>Inserted?}
    Confirm -->|Yes| Complete[Complete - Send Notification]
    Confirm -->|No| InputRemaining[Input Remaining Qty]
    InputRemaining --> NextComp[Assign Next Compartment]
    NextComp --> OpenDoor
    
    Complete --> Pickup[Kurir Pickup]
    Pickup --> Delivery[Delivery to Destination]
    Delivery --> End([Package Delivered])
    
    style AppLogin fill:#FFE5E5
    style WebLogin fill:#E5F5FF
    style GuestOrder fill:#FFF5E5
    style Complete fill:#E5FFE5
```

---

## 2. Channel Strategy Flow

```mermaid
flowchart LR
    User([User]) --> ChannelChoice{Access Point}
    
    ChannelChoice -->|Has Anteraja App| AndroidApp[Android App]
    ChannelChoice -->|No App| WebPlatform[Web Platform]
    
    AndroidApp --> MustLogin[Wajib Login]
    MustLogin --> ExistingUser{Existing User?}
    ExistingUser -->|Yes| Login[Login]
    ExistingUser -->|No| Register[Register]
    
    Login --> AppFeatures[Full Features<br/>- Auto-fill<br/>- 15% Discount<br/>- Points<br/>- Priority Support]
    Register --> AppFeatures
    
    WebPlatform --> WebChoice{Choose Experience}
    WebChoice -->|Quick| Guest[Guest Checkout]
    WebChoice -->|Better Deal| WebRegister[Register]
    
    Guest --> GuestFeatures[Standard Features<br/>- Manual Input<br/>- Normal Price<br/>- Basic Tracking]
    
    WebRegister --> WebLogin[Login]
    WebLogin --> RegisteredFeatures[Premium Features<br/>- Auto-fill<br/>- 15% Discount<br/>- Points<br/>- Priority Support]
    
    GuestFeatures --> Incentive[Show Registration<br/>Incentive]
    Incentive --> Convert{Convert?}
    Convert -->|Yes| WebRegister
    Convert -->|No| GuestFeatures
    
    style AndroidApp fill:#FFE5E5
    style WebPlatform fill:#E5F5FF
    style Guest fill:#FFF5E5
    style AppFeatures fill:#E5FFE5
    style RegisteredFeatures fill:#E5FFE5
```

---

## 3. Smart Box Drop Package Flow (Detailed)

```mermaid
flowchart TD
    Start([Customer Arrives at Smart Box]) --> ScanQR[Scan QR Code]
    
    ScanQR --> ValidateQR{QR Valid?}
    ValidateQR -->|No| ErrorQR[Show Error Message]
    ErrorQR --> End1([Contact Support])
    
    ValidateQR -->|Yes| AssignComp[Assign Compartment]
    AssignComp --> InputQty[Input Package Quantity]
    
    InputQty --> PrintAWB[Print AWB Labels]
    PrintAWB --> ShowInstruction[Show: Take Labels<br/>& Attach to Packages]
    ShowInstruction --> UserReady{User Tap 'Ready'}
    
    UserReady --> OpenDoor[Open Compartment Door]
    OpenDoor --> ShowTimer[Show Timer: 10 minutes]
    ShowTimer --> WaitInsert[Wait for User to<br/>Insert Packages]
    
    WaitInsert --> Timeout{Timeout?}
    Timeout -->|Yes| AutoClose[Auto-close Door]
    AutoClose --> Alert[Send Alert to User]
    Alert --> End2([Session Ended])
    
    Timeout -->|No| DoorClosed{Door Closed?}
    DoorClosed -->|No| WaitInsert
    
    DoorClosed -->|Yes| TakePhoto[Take Photo - Proof]
    TakePhoto --> AskConfirm[Ask: All Packages<br/>Inserted?]
    
    AskConfirm --> UserConfirm{User Response}
    UserConfirm -->|Yes, Done| UpdateStatus[Update Status:<br/>Packages in Box]
    UserConfirm -->|No, Continue| AskRemaining[Ask: How Many<br/>Packages Remaining?]
    
    AskRemaining --> InputRemaining[Input Remaining Qty]
    InputRemaining --> CheckCapacity{Compartment<br/>Available?}
    
    CheckCapacity -->|No| ShowError[Show: Box Full<br/>Use Another Box]
    ShowError --> End3([Redirect to Another Box])
    
    CheckCapacity -->|Yes| AssignNext[Assign Next Compartment]
    AssignNext --> ShowNext[Show: Continue to<br/>Compartment X]
    ShowNext --> OpenDoor
    
    UpdateStatus --> CreateTask[Create Pickup Task<br/>for Kurir]
    CreateTask --> SendNotif[Send Notification<br/>to Customer]
    SendNotif --> ShowSummary[Show Summary:<br/>X Packages in<br/>Y Compartments]
    ShowSummary --> End4([Drop Complete])
    
    style ValidateQR fill:#FFE5E5
    style UserConfirm fill:#FFF5E5
    style UpdateStatus fill:#E5FFE5
    style End4 fill:#E5FFE5
```

---

## 4. Kurir Pickup Flow

```mermaid
flowchart TD
    Start([Pickup Task Created]) --> AssignKurir[Auto-assign to<br/>Nearest Kurir]
    
    AssignKurir --> KurirNotif[Send Notification<br/>to Kurir App]
    KurirNotif --> KurirArrives[Kurir Arrives at<br/>Smart Box]
    
    KurirArrives --> ScanMaster[Scan Master QR]
    ScanMaster --> ValidateKurir{Validate Kurir}
    
    ValidateKurir -->|Invalid| ErrorKurir[Show Error]
    ErrorKurir --> End1([Access Denied])
    
    ValidateKurir -->|Valid| ShowList[Show List of<br/>Compartments with Packages]
    ShowList --> SelectComp[Kurir Select<br/>Compartment]
    
    SelectComp --> OpenComp[Open Compartment]
    OpenComp --> TakePackages[Kurir Takes Packages]
    TakePackages --> ScanAWB[Scan Each AWB]
    
    ScanAWB --> Validate{AWB Match?}
    Validate -->|No| AlertMismatch[Alert: AWB Not Found]
    AlertMismatch --> ScanAWB
    
    Validate -->|Yes| UpdateAWB[Update AWB Status:<br/>Picked Up]
    UpdateAWB --> MorePackages{More Packages<br/>in Compartment?}
    
    MorePackages -->|Yes| ScanAWB
    MorePackages -->|No| ConfirmComp[Confirm Compartment<br/>Complete]
    
    ConfirmComp --> CloseComp[Close Compartment]
    CloseComp --> UpdateComp[Update Compartment:<br/>Empty]
    
    UpdateComp --> MoreComp{More Compartments?}
    MoreComp -->|Yes| SelectComp
    MoreComp -->|No| FinalConfirm[Final Confirmation]
    
    FinalConfirm --> CheckCount{Expected vs<br/>Actual Match?}
    CheckCount -->|No| AlertDiscrepancy[Alert: Package<br/>Discrepancy]
    AlertDiscrepancy --> ReportIssue[Report to Operations]
    
    CheckCount -->|Yes| CompleteTask[Complete Pickup Task]
    CompleteTask --> SendNotifCustomers[Send Notification<br/>to All Customers]
    SendNotifCustomers --> TransportHub[Transport to Hub]
    TransportHub --> End2([Pickup Complete])
    
    style ValidateKurir fill:#FFE5E5
    style CheckCount fill:#FFF5E5
    style CompleteTask fill:#E5FFE5
    style End2 fill:#E5FFE5
```

---

## 5. Guest to Registered Conversion Flow

```mermaid
flowchart TD
    Start([Guest User<br/>Creates Order]) --> CompleteOrder[Order Complete]
    
    CompleteOrder --> ShowQR[Show QR Code]
    ShowQR --> ShowIncentive[Show Registration<br/>Incentive Card]
    
    ShowIncentive --> Highlight[Highlight Benefits:<br/>- 15% Discount<br/>- Auto-fill<br/>- Points<br/>- Priority Support]
    
    Highlight --> UserDecision{User Decision}
    UserDecision -->|Register Now| RegForm[Show Registration Form]
    UserDecision -->|Skip| TrackUsage[Track Guest Usage]
    
    RegForm --> QuickReg[Quick Registration:<br/>- Email<br/>- Password<br/>- Name<br/>- Phone]
    QuickReg --> VerifyEmail{Email Domain<br/>Campus?}
    
    VerifyEmail -->|Yes| AutoDiscount[Auto-apply 15% Discount]
    VerifyEmail -->|No| StandardReg[Standard Registration]
    
    AutoDiscount --> KTMPrompt[Prompt: Upload KTM<br/>for Verification]
    KTMPrompt --> UserUpload{Upload KTM?}
    UserUpload -->|Yes| ManualVerify[Manual Verification<br/>by Admin]
    UserUpload -->|No| BasicDiscount[10% Discount Only]
    
    ManualVerify --> Approved{Approved?}
    Approved -->|Yes| FullDiscount[15% Discount Activated]
    Approved -->|No| BasicDiscount
    
    FullDiscount --> RegisteredUser[Registered User<br/>with Full Benefits]
    BasicDiscount --> RegisteredUser
    StandardReg --> RegisteredUser
    
    TrackUsage --> CountOrders{Order Count}
    CountOrders -->|1st Order| Incentive1[Show: Save 15%<br/>Next Order]
    CountOrders -->|3rd Order| Incentive2[Show: Save Time<br/>with Auto-fill]
    CountOrders -->|5th Order| Incentive3[Show: Total Savings<br/>You Missed]
    
    Incentive1 --> RetryConvert{Convert?}
    Incentive2 --> RetryConvert
    Incentive3 --> RetryConvert
    
    RetryConvert -->|Yes| RegForm
    RetryConvert -->|No| TrackUsage
    
    RegisteredUser --> End([Conversion Success])
    
    style UserDecision fill:#FFF5E5
    style RegisteredUser fill:#E5FFE5
    style End fill:#E5FFE5
```

---

## 6. Revenue Flow

```mermaid
flowchart LR
    Users([Users]) --> Channel{Channel}
    
    Channel -->|Android App| AppUsers[App Users<br/>Wajib Login]
    Channel -->|Web| WebUsers{Web Users}
    
    WebUsers -->|40%| GuestUsers[Guest Users]
    WebUsers -->|60%| RegUsers[Registered Users]
    
    AppUsers --> AppRevenue[Revenue:<br/>Rp 12,750/paket<br/>8-12 paket/bulan]
    RegUsers --> RegRevenue[Revenue:<br/>Rp 12,750/paket<br/>8-12 paket/bulan]
    GuestUsers --> GuestRevenue[Revenue:<br/>Rp 15,000/paket<br/>3-5 paket/bulan]
    
    AppRevenue --> TotalRevenue[Total Monthly Revenue]
    RegRevenue --> TotalRevenue
    GuestRevenue --> TotalRevenue
    
    TotalRevenue --> Costs[Operational Costs:<br/>- Hardware<br/>- Maintenance<br/>- Pickup<br/>- Support]
    
    Costs --> Profit[Monthly Profit:<br/>Rp 10.54 juta/unit]
    
    Profit --> Reinvest{Reinvestment}
    Reinvest --> Expansion[Expansion:<br/>More Smart Boxes]
    Reinvest --> Features[New Features]
    Reinvest --> Marketing[Marketing]
    
    Expansion --> Growth([Business Growth])
    Features --> Growth
    Marketing --> Growth
    
    style TotalRevenue fill:#E5FFE5
    style Profit fill:#E5FFE5
    style Growth fill:#E5FFE5
```

---

## 7. Operations Flow

```mermaid
flowchart TD
    Start([Daily Operations]) --> Morning[Morning Pickup<br/>09:00]
    
    Morning --> CheckBoxes[Check All Smart Boxes<br/>Status]
    CheckBoxes --> Issues{Issues Found?}
    
    Issues -->|Yes| Maintenance[Dispatch Technician]
    Issues -->|No| Afternoon[Afternoon Pickup<br/>14:00]
    
    Maintenance --> FixIssue[Fix Issue]
    FixIssue --> UpdateStatus[Update Box Status]
    UpdateStatus --> Afternoon
    
    Afternoon --> Evening[Evening Pickup<br/>18:00]
    Evening --> DailyReport[Generate Daily Report]
    
    DailyReport --> Metrics[Track Metrics:<br/>- Packages Processed<br/>- Uptime<br/>- User Satisfaction<br/>- Revenue]
    
    Metrics --> Analysis{Performance<br/>Analysis}
    
    Analysis -->|Below Target| ActionPlan[Create Action Plan]
    Analysis -->|On Target| Continue[Continue Operations]
    Analysis -->|Above Target| Optimize[Optimize & Scale]
    
    ActionPlan --> Implement[Implement Improvements]
    Implement --> Monitor[Monitor Results]
    
    Optimize --> AddCapacity[Add More Boxes]
    AddCapacity --> Monitor
    
    Continue --> Monitor
    Monitor --> End([End of Day])
    
    style Metrics fill:#E5F5FF
    style Optimize fill:#E5FFE5
    style End fill:#E5FFE5
```

---

## 8. Decision Tree: Package Size Handling

```mermaid
flowchart TD
    Start([Customer Arrives<br/>with Packages]) --> InputQty[Input Quantity]
    
    InputQty --> PrintAWB[Print AWB]
    PrintAWB --> OpenComp[Open Compartment A]
    OpenComp --> TryInsert[Try to Insert Packages]
    
    TryInsert --> FitCheck{All Packages<br/>Fit?}
    
    FitCheck -->|Yes| CloseA[Close Compartment A]
    CloseA --> ConfirmA[Confirm: All In?]
    ConfirmA -->|Yes| Done[Complete]
    
    FitCheck -->|No| PartialInsert[Insert What Fits]
    PartialInsert --> ClosePartial[Close Compartment A]
    ClosePartial --> AskContinue[Ask: All Packages In?]
    
    AskContinue -->|Yes| Done
    AskContinue -->|No| InputRemaining[Input Remaining Qty]
    
    InputRemaining --> CheckAvailable{Compartment<br/>Available?}
    
    CheckAvailable -->|No| BoxFull[Show: Box Full]
    BoxFull --> Suggest[Suggest:<br/>- Use Another Box<br/>- Request Pickup<br/>- Go to Counter]
    Suggest --> UserChoice{User Choice}
    UserChoice -->|Another Box| FindBox[Find Another Box]
    UserChoice -->|Pickup| RequestPickup[Request Pickup]
    UserChoice -->|Counter| GoCounter[Go to Counter]
    
    CheckAvailable -->|Yes| OpenNext[Open Compartment B]
    OpenNext --> InsertRemaining[Insert Remaining]
    InsertRemaining --> CloseB[Close Compartment B]
    CloseB --> ConfirmB[Confirm: All In?]
    
    ConfirmB -->|Yes| Done
    ConfirmB -->|No| CheckMore{More<br/>Compartments?}
    CheckMore -->|Yes| InputRemaining
    CheckMore -->|No| BoxFull
    
    Done --> Summary[Show Summary:<br/>Packages in<br/>Multiple Compartments]
    Summary --> End([Drop Complete])
    
    style FitCheck fill:#FFF5E5
    style CheckAvailable fill:#FFF5E5
    style Done fill:#E5FFE5
    style End fill:#E5FFE5
```

---

## Legend

- 🟢 **Green**: Success/Complete states
- 🟡 **Yellow**: Decision points
- 🔴 **Red**: Error/Issue states
- ⚪ **White**: Process steps

---

**Document Version**: 1.0
**Last Updated**: November 2025
**Owner**: Product Team - Anteraja Campus HUB

**Note**: These flowcharts can be rendered using Mermaid-compatible tools like:
- GitHub (native support)
- Mermaid Live Editor (https://mermaid.live)
- VS Code with Mermaid extension
- Notion, Confluence, etc.
