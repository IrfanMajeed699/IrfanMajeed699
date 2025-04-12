# Hi, I'm Irfan Majeed

Although having a background of Industrial & Manufacturing Engineering and a solid job experience of more than sixteen years in a public sector organization 
at different managerial posts, I learned Flutter framework at my own with self-driven passion and motivation. I am ambitious to shift my career 
from engineering to a full time mobile app developer in the long run and willing to enhance my skills into other software development fields 
if required by the organization.
I transitioned myself into software development in January 2024 and independently developed multiple complex production-ready Flutter applications, 
including a real-time collaborative Shopping Checklist App integrated with Firebase.
Initially, I am seeking a part-time mobile development opportunity to further sharpen my skills and contribute to impactful projects.

# Skills
- Flutter (Dart), Firebase (Real-time DB, Authentication, Cloud Functions in Python),
SQLite Database, Git & GitHub, APIs, JSON, GetX (State Management & Navigation)
Debugging & Troubleshooting.
- By using online blog’s assistance and AI tools, Root Cause Analysis of the problems & errors for troubleshooting & debugging while in debug and 
release modes (Build APK’s, APK Splits & App Bundle files).

# Projects Overview

## 🛍 Shopping Checklist App
- Complete Ready to Use Application for Family Shopping / Group Tasking.
- Used Firebase, SQLite, GetX State Management & Navigation, Cloud Function (Python).
- User has the Create account (Sign Up), Log In, Log Out and Delete Account options.
- User has the options to prepare multiple “Private” or “Sharable” Lists.
- Addition of Multiple Items into each list with the options to add Quantity, Unit, Cost & Currency.
- Options to Rename, Check, Uncheck, delete items or Lists.
- Shareable Lists would be shared across multiple devices in Real Time to family, friends or Co-Workers having same Username & Password (Through Firebase Real-time Database).
- Private or Sharable Lists can be retrieved at a Single Tap.
- Summary of Total / Checked (Purchased) Items and Total / Consumed Cost at each page.
- Email Verification and ‘Forgot Password’ Options.
- Cloud function for Account & Data Deletion from Firebase
- Offline Capabilities with SQLite.
- Auto Detecting User’s Online Status (Status is shown in UI and updated after every five Seconds).
- Sharing of a Specific Page through Medias like WhatsApp & Email etc. with Checked / Unchecked status and with Quantity & Cost Display.
- Privacy Policy & Terms of Services.
- Responsive UI for multiple devices, tabs, portrait and landscape Modes
- Attractive and User friendly UI features with Splash screen, Light / Dark Modes, Customized Color Contrasts, Bottom Sheets, Flutter Toasts,
  Snack bars, Tool Tips, Dialog & Alert Boxes etc.

### 📅 Task Planning App (Work in Progress – 70%)
- Used GetX Controller & SQLite Database.
- User has the option to Add and Schedule Tasks with Reminders.
- Plan Daily/Weekly/Monthly Activities.
- UI Categorization as Today’s, Previous & Future Tasks based on the Scheduled Dates.
- Categorization of Tasks in UI as All, Family, Office, Birthdays, Weddings, Friends, etc.
- Local Notifications (Scheduled Reminders).
- Calendar Page with marked dates of scheduled Tasks.


#### 📲 Learning Projects
- E-Commerce App (Used a prebuild JSON file for displaying Title, Subtitle, Description, Picture, Price & Currency of an
Item in the UI for all enlisted items and at individual item’s page with ‘Add to Cart’ option for the User having Total quantity and Cost of added Items).
- Weather App (Using ‘openweathermap.org’ API)
- Food Recipe App (Using API ‘edamam.com’)
- News App (Using API ‘newsapi.org’)
- BMI Calculator


# 📷 App Screenshots (Shopping Checklist App)
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Headings%20Page.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Items%20Page.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Splash_Screen.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Qty%20&%20Cost%20Dialog.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Predictive%20Text.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Share_Media.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Sign_In%20Page.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Sign_Up%20Page.jpg?raw=true
https://github.com/IrfanMajeed699/IrfanMajeed699/blob/main/Initial%20Heading%20Page.jpg?raw=true

#  Code Highlights
```dart
### For Main.dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  await DatabaseHelper.init();
  await GetStorage.init();
  log('Initializing controllers');
  Get.put(HeadingController()); // Depends on ItemsController
  Get.put(ItemsController());
  Get.put(
      ChecklistController()); // Depends on HeadingController and ItemsController
  Get.put(ThemeServices());
    runApp(const MyApp());
}
### For Method to Load Headings
  Future<void> loadChecklistHeadings() async {
    log("Loading checklist headings...");
    try {
  //    log('Loading checklist headings from SQLite...');
      // Load headings from the database (both user-added and deleted ones)
      final dbHeadings =
          await DatabaseHelper.instance.getHeadingsCreatedByUser();
  //    log('Retrieved user-created headings: $dbHeadings');
      // Add all user-created headings into catList
      userCatList.addAll(dbHeadings);
      // Populate headingList from database for display in UI
      headingList.value = await DatabaseHelper.instance.getChecklistHeadings();
      //    log('Fetched checklist headings for UI display: $headingList and Heading ${headingList[3].heading} by ${headingList[3].addedByUserId}');
      // Sort headingList: Incomplete first, then Completed
      // Refresh the headingList to update the UI
      //  headingList.refresh();
      checkStatus.clear();
      for (var heading in headingList) {
        completionStatus[heading.id!] = heading.isCompleted;
        checkStatus[heading.id!] = false;
        //    log('Set completionStatus and checkStatus for heading ID: ${heading.id},Heading:${heading.heading}, isCompleted: ${heading.isCompleted}, FirebaseID: ${heading.firebaseId} , User: ${heading.addedByUserId}');
      }
  //    isHeadingLoading.value = false;
      //   log("HeadingLoading completed.'isHeadingLoading' set to false.");
    } catch (e) {
      //   log('Error loading checklist headings: $e');
    }
  }
  #### For Model Page
  class ChecklistItem {
  int? id; // Local database ID
  String headingId; // ID of the associated heading
  String title; // Title of the item
  bool isItemChecked; // Check status of the item
  int isDeleted; // Deletion status (0 = not deleted, 1 = deleted)
   bool needsSync; // New field to track if the record needs to sync
  String? quantity; // Item quantity
  String? unit; // Unit of measurement
  String? currency; // Currency for the cost
  String? cost; // Cost of the item
  String firebaseId; // Firebase ID for syncing with Firebase (now required)

  ChecklistItem({
    this.id,
    required this.headingId,
    required this.title,
    this.isItemChecked = false,
    this.isDeleted = 0,
    this.needsSync = false, // Default to false (already synced)
    this.quantity,
    this.unit,
    this.currency,
    this.cost,
   this.firebaseId = '', // Set default as empty string to avoid null issues
  });


  // Convert ChecklistItem instance to JSON format for Firebase
Map<String, dynamic> toJson() {
  return {
    'id': id,
    'headingId': headingId,
    'title': title.isNotEmpty ? title : 'Untitled Item',
    'isItemChecked': isItemChecked ? 1 : 0,
    'isDeleted': isDeleted,
    'needsSync': needsSync ? 1 : 0,
    'quantity': quantity ?? '',
    'unit': unit ?? '',
    'currency': currency ?? '',
    'cost': cost ?? '',
    'firebaseId': firebaseId.isNotEmpty ? firebaseId : '',
  };
}



  // Create ChecklistItem instance from JSON data
  static ChecklistItem fromJson(Map<String, dynamic> json) {
    return ChecklistItem(
      id: json['id'] as int?,
      headingId: json['headingId'] as String, // Ensure this field isn't null
      title: json['title'] as String? ?? '',
      isItemChecked: json['isItemChecked'] == 1,
      isDeleted: json['isDeleted'] as int,
      needsSync: json['needsSync'] == 1, // Track sync status
      quantity: json['quantity'] as String?,
      unit: json['unit'] as String?,
      currency: json['currency'] as String?,
      cost: json['cost'] as String?,
       firebaseId: json['firebaseId'] ?? '', // Ensure non-null
    );
  }
}


# 📞 Contact Me
📧 irsum556@gmail.com  
📱 +92-300-4070847  
🔗 WhatsApp: +92-310-1509209  
🔗 [LinkedIn – Coming Soon]
