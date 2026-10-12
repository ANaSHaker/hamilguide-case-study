<p align="center"><img src="assets/banner.png" alt="HamilGuide — Pregnancy Tracker" width="100%"></p>
<h1 align="center">HamilGuide · دليل الحوامل</h1>
<p align="center"><b>A pregnancy companion for Arabic-speaking mothers — due-date calculator, week-by-week guide, health trackers and baby tools. 100K+ installs.</b></p>
<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS%20%7C%20iPad-0F172A?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/Flutter-3.x-02569B?style=flat-square&logo=flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/State-Bloc%20%2F%20Cubit-8B5CF6?style=flat-square" alt="Bloc / Cubit">
  <img src="https://img.shields.io/badge/Backend-Firebase-FFCA28?style=flat-square&logo=firebase&logoColor=black" alt="Firebase">
  <img src="https://img.shields.io/badge/Installs-100K%2B-EC4899?style=flat-square&logo=googleplay&logoColor=white" alt="100K+ installs">
  <img src="https://img.shields.io/badge/Case%20Study-portfolio-111827?style=flat-square" alt="Case Study">
</p>
<p align="center">
  <a href="https://play.google.com/store/apps/details?id=com.dalil.elhwamelss"><img src="https://img.shields.io/badge/Google%20Play-Get%20it-34A853?style=for-the-badge&logo=googleplay&logoColor=white" alt="Get it on Google Play"></a>
  <a href="https://apps.apple.com/app/id6443834135"><img src="https://img.shields.io/badge/App%20Store-Download-0D96F6?style=for-the-badge&logo=appstore&logoColor=white" alt="Download on the App Store"></a>
</p>

> ℹ️ Case study — the source code is proprietary to the client and is not included.

## Overview

**HamilGuide** (دليل الحوامل — "the pregnant woman's guide") helps expecting mothers follow their pregnancy day by day: when the baby is due, what is happening this week, how their blood pressure, sugar and weight are trending, how often the baby kicks — and, after birth, how the baby is feeding, sleeping and developing.

| | |
|---|---|
| **Audience** | Arabic-speaking mothers (Egypt, Gulf, wider MENA) — UI also in English, French, Spanish and German |
| **Platforms** | Android, iPhone and iPad from one Flutter codebase |
| **My role** | Flutter developer — full mobile delivery: features, Firebase integration, releases on both stores |
| **Timeline** | On the App Store since October 2022; continuously updated (store version 28.x) |

## The challenge

- **Medical dates are easy to get wrong.** Mothers need a due date they can trust, calculated the way doctors do it — and many users track dates in the **Hijri** calendar, not only Gregorian.
- **Too many apps for one journey.** Mothers were juggling a calculator, a notes app, reminders and forums. HamilGuide puts the tools in one place.
- **Health data has to be quick to log** — a kick session or a sugar reading should take seconds, and be there on the next visit to the doctor.
- **Free for users.** The product is ad-supported, so ads had to fit in without breaking the experience.

## Highlights

**🤰 Pregnancy**
- Due-date calculator from the first day of the last period — **Gregorian or Hijri**
- Current week / month / day, days remaining and trimester progress
- Week-by-week guide (weeks 1–40) about the mother and the baby
- Tips, warning signs and common pregnancy myths; curated video library (YouTube)

**📈 Health trackers**
- **Kick counter** with timed sessions and averages
- Blood pressure, blood sugar and weight logs with charts and status labels
- Alarms / reminders for doctor visits, vitamins and medication (local notifications)

**👶 Baby & family**
- Baby-names explorer (boys / girls, meanings) with favourites
- Baby tracker after birth: feeding, health, language and social development, notes
- Notes and to-dos with *new / done / archived* tabs

**💬 Community & product**
- Community chat rooms backed by Cloud Firestore
- 5 languages (Arabic, English, French, Spanish, German) with `easy_localization`
- Phone, tablet and iPad layouts; dark-mode toggle
- In-app review prompts, share, push notifications, ad monetisation

## Screens

<p align="center"><img src="assets/screens-play.png" alt="Google Play screenshots" width="100%"></p>
<p align="center"><sub>Current Google Play release: home, pregnancy calculator, health follow-up, kick counter, baby tracker, baby names, notes & reminders, community.</sub></p>

<p align="center"><img src="assets/screens-ios.png" alt="iOS build screenshots" width="100%"></p>
<p align="center"><sub>iOS build from the App Store listing: calculator input, result, follow-up tools, weeks guide, baby names and articles.</sub></p>

## Signature feature — pregnancy calculator + daily trackers

<p align="center"><img src="assets/feature-tracking.png" alt="Pregnancy calculator and kick counter" width="100%"></p>

**How it works**
1. The mother picks a calendar type (**Gregorian / Hijri**) and enters the first day of her last period with year / month / day pickers (Hijri months are handled with `hijri_picker`).
2. The app computes the expected delivery date, the current week and day, the remaining time and the trimester, and shows them as a progress ring.
3. Daily trackers (kicks, pressure, sugar, weight) are posted to the HamilGuide REST API through Dio and listed back as a history with averages and status labels (normal / high / low).
4. Reminders are scheduled with local notifications, so they fire even without a network connection.

## Architecture & stack

```mermaid
flowchart LR
  UI[Screens<br/>calculator · weeks · trackers · baby · notes · chat]
  subgraph State["Cubits"]
    AC[AppCubit]
    AU[AuthCubit]
    NC[NoteCubit]
    AL[AlarmCubit]
  end
  SQL[(SQLite<br/>notes · to-dos)]
  API[(REST API<br/>weeks · trackers · content)]
  LN[Local notifications<br/>alarms]
  FS[(Cloud Firestore<br/>community chat)]
  FA[Firebase Auth · Storage]
  FM[FCM · Crashlytics]
  YT[YouTube player]
  ADS[Ads · in-app review]

  UI --> AC & AU & NC & AL
  NC --> SQL
  AC --> API
  AL --> LN
  AC --> FS
  AU --> FA
  AC --> YT
  AC --> ADS
  UI -.-> FM
```

| Layer | Tech |
|---|---|
| UI | Flutter, `sizer` + `flutter_screenutil` (phone / tablet), Google Fonts, Lottie, curved navigation bar, clay containers |
| State | `flutter_bloc` — App, Auth, Note and Alarm cubits |
| Networking | Dio — REST API for weeks content (per language), kicks, pressure, sugar and weight logs |
| Local data | `sqflite` (notes and to-dos), shared-preferences cache |
| Cloud | Firebase Auth, Cloud Firestore (chat), Firebase Storage, FCM, Crashlytics |
| Dates | Gregorian + Hijri calculation, `hijri_picker` |
| Media | `youtube_player_iframe`, cached network images |
| Notifications | `flutter_local_notifications` (reminders), FCM (push) |
| i18n | `easy_localization` — ar-EG, en-US, fr-FR, es-ES, de-DE |
| Growth | `in_app_review`, share, ad SDK, Shorebird code push |

## Engineering notes

- **Two calendars, one result.** The calculator accepts Gregorian or Hijri input; month lists for both calendars are localised, and the result screen always shows the due date, elapsed weeks / days and what is left.
- **Server-backed trackers, local notes.** Kicks, pressure, sugar and weight go to the backend so the history follows the user across devices, while personal notes and to-dos stay in on-device SQLite. Week-by-week content is fetched in the user's language (Arabic, English, French …).
- **Reliable reminders.** Doctor visits, vitamins and medication are scheduled as local notifications through a dedicated Alarm cubit, independent of the backend.
- **Five languages, RTL and LTR.** All strings (~350 keys) go through `easy_localization`; layouts switch direction with the locale.
- **Phone, tablet and iPad.** Sizing is relative (`sizer` / `screenutil`), and the same build ships iPad screenshots on the App Store.
- **Shipping fast at scale.** Crashlytics for crash monitoring and Shorebird for code-push patches let fixes reach a 100K+ user base without waiting for a full store review.

## Results

- **100K+ installs** on Google Play, rated **4.2★** (Google Play listing).
- **4.3★ from 88 ratings** on the App Store (Health & Fitness · Medical).
- Live on both stores since **2022**, still maintained (App Store version 28.2.1).

## بالعربي

**دليل الحوامل (HamilGuide)** تطبيق لمتابعة الحمل يومًا بيوم للأم العربية.
يحسب موعد الولادة من أول يوم في آخر دورة بالتقويم **الميلادي أو الهجري**، ويعرض الأسبوع الحالي والمتبقي ومعلومات كل أسبوع.
فيه عداد ركلات الجنين، ومتابعة الضغط والسكر والوزن، ومنبهات لمواعيد الطبيب والفيتامينات، وأسماء المواليد، ومتابعة الطفل بعد الولادة، ودردشة مجتمع.
متاح بخمس لغات على Android وiPhone وiPad، وتخطى **100 ألف تحميل** على Google Play بتقييم 4.2.
دوري: تطوير التطبيق بـ Flutter بالكامل — الميزات، تكامل Firebase، والنشر على المتجرين.

## Links

- Google Play — https://play.google.com/store/apps/details?id=com.dalil.elhwamelss
- App Store — https://apps.apple.com/app/id6443834135

---
<h3 align="center">Built by Mahmoud Shaker — Senior Flutter Engineer</h3>
<p align="center">
  <a href="https://wa.me/201140279111"><img src="https://img.shields.io/badge/WhatsApp-Chat-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp"></a>
  <a href="https://www.linkedin.com/in/anashaker/"><img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="https://anashaker.github.io/shaker_portfolio/"><img src="https://img.shields.io/badge/Portfolio-Visit-111827?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Portfolio"></a>
  <a href="mailto:mahmoudshaker382001@gmail.com"><img src="https://img.shields.io/badge/Email-Hire%20me-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"></a>
  <a href="https://khamsat.com/user/sheko201566"><img src="https://img.shields.io/badge/Khamsat-Services-F39C12?style=for-the-badge" alt="Khamsat"></a>
  <a href="https://mostaql.com/u/ShekoShaker"><img src="https://img.shields.io/badge/Mostaql-Freelance-2386C8?style=for-the-badge" alt="Mostaql"></a>
  <a href="https://github.com/ANaSHaker"><img src="https://img.shields.io/badge/GitHub-More%20work-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
</p>
