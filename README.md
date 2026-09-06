# 🛍️ Swift Mobile App (سويفت)

[![Flutter](https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)](https://supabase.com)
[![Bloc](https://img.shields.io/badge/State_Management-Bloc-blue?style=for-the-badge)](https://bloclibrary.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

**Swift Mobile App** is a modern, full-featured multi-vendor e-commerce platform built with **Flutter** and powered by **Supabase**. The platform offers a dual-role experience seamlessly connecting **Clients (Buyers)** and **Sellers (Merchants)** with a localized, RTL-first Arabic interface.

Designed following **Clean Architecture** principles and utilizing the **BLoC/Cubit** state management pattern, Swift is engineered for high performance, modularity, and scalability.

---

## 📑 Table of Contents

- [Key Features](#-key-features)
  - [Client Portal (المشتري)](#-client-portal-المشتري)
  - [Seller Portal (التاجر)](#-seller-portal-التاجر)
  - [System & Core Features](#-system--core-features)
- [Architecture & Tech Stack](#-architecture--tech-stack)
- [Project Directory Structure](#-project-directory-structure)
- [Database & Storage Schema](#-database--storage-schema)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation & Setup](#installation--setup)
  - [Configuring Credentials (`app_keys.dart`)](#configuring-credentials-app_keysdart)
  - [Running the App](#running-the-app)
- [Design System & Palette](#-design-system--palette)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Key Features

### 🛒 Client Portal (المشتري)
- **Interactive Onboarding:** Engaging introduction flow for first-time users.
- **Authentication & Profiles:** Secure email/password login and signup with session persistence.
- **Product Discovery & Search:**
  - Dynamic category-based browsing.
  - Fast search with instant filtering.
  - Highlighting featured and popular products.
- **Product Details & Customization:**
  - Rich image galleries with cached network images.
  - Multi-variant attribute selection (sizes, colors, specifications).
  - Customer ratings and review breakdown.
- **Cart & Checkout Management:**
  - Real-time cart calculations and quantity modifications.
  - Seamless order placement workflow.
- **Order Tracking:** Detailed order history with live item status inspection.
- **Account Settings:** Profile editing, phone number updates, and terms and privacy policy review.

### 🏬 Seller Portal (التاجر)
- **Seller Onboarding & Verification:**
  - Dedicated seller registration workflow with verification document upload (`id.images`).
  - Account status lifecycle: `pending_sellers`, `rejected_sellers`, and approved `sellers`.
- **Product Management (CRUD):**
  - **Add Products:** Assign categories, define dynamic attributes/specifications, and upload multiple product images.
  - **Client-Side Image Optimization:** Image compression before upload using `flutter_image_compress` to optimize bandwidth.
  - **Edit & Update:** Instant price, description, and attribute updates.
  - **Deletion with Cleanup:** Automatic deletion of product listings and associated Supabase storage files.
- **Reviews & Feedback:** View and monitor client reviews and product ratings.
- **Merchant Profile:** Manage business details and store profile.

### ⚙️ System & Core Features
- **Arabic & RTL Native:** Custom Arabic typography (`duco` font) and natural right-to-left layout direction.
- **Responsive Across Screens:** Responsive scaling with `flutter_screenutil` (base design 375x812) and `device_preview` support.
- **Remote In-App Update Engine:** Built-in `AppUpdateService` checking `app_config` in Supabase for mandatory (force) or optional updates with store redirection.
- **Robust Error Handling:** Functional error and failure handling using `dartz` (`Either<Failure, T>`).

---

## 🏗 Architecture & Tech Stack

The application strictly follows **Clean Architecture** organized with a **feature-first** structure:

```
Domain Layer (Entities, Use Cases, Repository Contracts)
       ▲
       │
Data Layer (Models, Data Sources, Repository Implementations)
       ▲
       │
Presentation Layer (BLoC / Cubits, Screens, Reusable Widgets)
```

### Core Technologies:
| Component | Technology / Package |
|---|---|
| **Framework** | [Flutter](https://flutter.dev) (Dart SDK `^3.7.0`) |
| **Backend & Auth** | [Supabase Flutter](https://pub.dev/packages/supabase_flutter) |
| **State Management** | [flutter_bloc](https://pub.dev/packages/flutter_bloc) / [bloc](https://pub.dev/packages/bloc) |
| **Dependency Injection** | [get_it](https://pub.dev/packages/get_it) |
| **Functional Error Handling** | [dartz](https://pub.dev/packages/dartz) |
| **Local Storage** | [shared_preferences](https://pub.dev/packages/shared_preferences) |
| **Responsive UI** | [flutter_screenutil](https://pub.dev/packages/flutter_screenutil), [device_preview](https://pub.dev/packages/device_preview) |
| **Image Compression** | [flutter_image_compress](https://pub.dev/packages/flutter_image_compress) |
| **Icons & Media** | [flutter_svg](https://pub.dev/packages/flutter_svg), [font_awesome_flutter](https://pub.dev/packages/font_awesome_flutter), [google_nav_bar](https://pub.dev/packages/google_nav_bar) |

---

## 📁 Project Directory Structure

```text
lib/
├── app_keys.dart                # Supabase project URL & Anon Key (git-ignored)
├── constants.dart               # App-wide storage keys and constants
├── main.dart                    # Application entrypoint & role-based routing
├── core/                        # Shared reusable application foundation
│   ├── cubits/                  # Global cubits (UserCubit, EditProfileDetailsCubit)
│   ├── entities/                # Shared business entities (ProductEntity, etc.)
│   ├── errors/                  # Custom Failure and Exception classes
│   ├── helper_classes/          # Utility helpers
│   ├── helper_functions/        # Dialogs, snackbars, and route generator
│   ├── models/                  # Base models
│   ├── repos/                   # Shared repositories (ImageRepo, ProfileRepo)
│   ├── services/                # Supabase DB, Auth, Storage, and Update services
│   ├── utils/                   # AppColors, AppFontStyles, AppImages
│   └── widgets/                 # Common reusable buttons, cards, text fields
└── features/                    # Feature modules (Feature-First Clean Architecture)
    ├── onboarding/              # Welcome and onboarding screens
    ├── client/                  # Client features
    │   ├── auth/                # Client login, registration, and models
    │   ├── home/                # Main home feed, categories, and search
    │   ├── product_details/     # Product view, attributes, reviews
    │   ├── cart/                # Shopping cart and checkout
    │   ├── order/               # Order history and order item tracking
    │   └── profile/             # Client profile settings
    └── seller/                  # Seller features
        ├── auth/                # Seller signup (document upload), login
        ├── add_product/         # Product creation and category assignment
        ├── home/                # Seller product inventory and review monitoring
        └── profile/             # Seller profile management
```

---

## 🗄 Database & Storage Schema

Swift interacts with the following Supabase resources:

### Database Tables:
- `users`: Registered client profiles.
- `sellers`: Approved merchant accounts.
- `pending_sellers`: Merchant registration requests pending review.
- `rejected_sellers`: Denied seller applications.
- `products`: Master product catalog.
- `categories`: Available product categories.
- `product_attributes`: Dynamic product attribute keys (e.g., Color, Size).
- `atrributes_values`: Attribute values assigned to individual products.
- `cart_items`: Items currently stored in customer shopping carts.
- `orders`: Master order records.
- `order_items`: Individual items associated with placed orders.
- `reviews`: Product reviews and ratings submitted by clients.
- `app_config`: Application version control and force update rules.

### Storage Buckets:
- `id.images`: Seller verification and identity proof documents.
- `product.images`: Product imagery uploaded by merchants.

---

## 🚀 Getting Started

### Prerequisites
- [Flutter SDK](https://docs.flutter.dev/get-started/install) (`v3.22.0+` recommended, Dart `^3.7.0`)
- An active [Supabase](https://supabase.com/) project with the required schema and storage buckets.
- Android Studio / Xcode / VS Code with Flutter extensions.

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/M0KK-11/Swift.git
   cd Swift
   ```

2. **Install project dependencies:**
   ```bash
   flutter pub get
   ```

### Configuring Credentials (`app_keys.dart`)

For security, the API credentials file is excluded from Git. Create a new file at `lib/app_keys.dart`:

```dart
// lib/app_keys.dart

class AppKeys {
  static const String url = 'YOUR_SUPABASE_PROJECT_URL';
  static const String annon = 'YOUR_SUPABASE_ANON_KEY';
}
```

> [!IMPORTANT]
> Ensure that both `id.images` and `product.images` buckets are configured in your Supabase Storage with appropriate public/read policies.

### Running the App

- **Run in Debug mode:**
  ```bash
  flutter run
  ```

- **Run on a specific device/emulator:**
  ```bash
  flutter devices
  flutter run -d <DEVICE_ID>
  ```

- **Build APK for Android:**
  ```bash
  flutter build apk --release
  ```

- **Build for iOS:**
  ```bash
  flutter build ipa --release
  ```

---

## 🎨 Design System & Palette

Swift utilizes a warm, premium color palette tailored for e-commerce:

| Role | Color Code | Preview |
|---|---|---|
| **Primary Color** | `#B31E1C` (Crimson) | `rgb(179, 30, 28)` |
| **Secondary Color** | `#1E2E52` (Navy Blue) | `rgb(30, 46, 82)` |
| **Background Color** | `#F9F3E9` (Warm Cream) | `rgb(249, 243, 233)` |

- **Typography:** Duco Font Family (`duCo_WHeadline16.ttf`)
- **Layout Direction:** RTL (Right-to-Left)

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve the project:

1. Fork the project.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
