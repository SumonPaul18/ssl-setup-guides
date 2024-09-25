## .pem ফাইল তৈরি করার নির্দেশাবলী

SSL .pem ফাইল (সংযুক্ত সার্টিফিকেট কনটেইনার ফাইল) প্রায়শই সার্টিফিকেট ইনস্টলেশনের জন্য প্রয়োজন হয় যখন একাধিক সার্টিফিকেট একই ফাইল হিসাবে আমদানি করা হয়। এই নিবন্ধে সার্টিফিকেট ইনস্টলেশনের জন্য বিভিন্ন .pem ফাইল তৈরির বিভিন্ন সেট নির্দেশাবলী রয়েছে।

### সম্পূর্ণ TLS/SSL সার্টিফিকেট ট্রাস্ট চেইন সহ একটি .pem ফাইল তৈরি করুন

* আপনার CertCentral অ্যাকাউন্টে, সার্টিফিকেটের অর্ডার বিবরণ পৃষ্ঠায়, আপনার ইন্টারমিডিয়েট (DigiCertCA.crt), রুট (TrustedRoot.crt) এবং প্রাইমারি সার্টিফিকেট (আপনার_ডোমেইন_নাম.crt) ডাউনলোড করুন।
* একটি টেক্সট এডিটর (যেমন নোটপ্যাড) খুলুন এবং নিম্নলিখিত ক্রমে প্রতিটি সার্টিফিকেটের সম্পূর্ণ বডি একই টেক্সট ফাইলে পেস্ট করুন:
    * প্রাইমারি সার্টিফিকেট - আপনার_ডোমেইন_নাম.crt
    * ইন্টারমিডিয়েট সার্টিফিকেট - DigiCertCA.crt
    * রুট সার্টিফিকেট - TrustedRoot.crt
* কিছু সার্ভারের জন্য আপনাকে .pem ফাইলে সার্টিফিকেটগুলি বিপরীত ক্রমে যোগ করতে হতে পারে:
    * রুট সার্টিফিকেট
    * ইন্টারমিডিয়েট সার্টিফিকেট
    * প্রাইমারি সার্টিফিকেট
* প্রতিটি সার্টিফিকেটের শুরু এবং শেষ ট্যাগ অন্তর্ভুক্ত করতে ভুলবেন না। ফলাফলটি এরকম হওয়া উচিত:
```
-----BEGIN CERTIFICATE-----
আপনার প্রাইমারি TLS/SSL সার্টিফিকেট: আপনার_ডোমেইন_নাম.crt
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
আপনার ইন্টারমিডিয়েট সার্টিফিকেট: DigiCertCA.crt
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
আপনার রুট সার্টিফিকেট: TrustedRoot.crt
-----END CERTIFICATE-----
```
* সংযুক্ত ফাইলটি আপনার_ডোমেইন_নাম.pem হিসাবে সেভ করুন।
* .pem ফাইলটি এখন ব্যবহারের জন্য প্রস্তুত।

### TLS/SSL সার্ভার এবং ইন্টারমিডিয়েট সার্টিফিকেট সহ একটি .pem ফাইল তৈরি করুন

* আপনার CertCentral অ্যাকাউন্টে, সার্টিফিকেটের অর্ডার বিবরণ পৃষ্ঠায়, আপনার ইন্টারমিডিয়েট (DigiCertCA.crt) এবং প্রাইমারি সার্টিফিকেট (আপনার_ডোমেইন_নাম.crt) ডাউনলোড করুন।
* একটি টেক্সট এডিটর (যেমন নোটপ্যাড) খুলুন এবং নিম্নলিখিত ক্রমে প্রতিটি সার্টিফিকেটের সম্পূর্ণ বডি একই টেক্সট ফাইলে পেস্ট করুন:
    * প্রাইমারি সার্টিফিকেট - আপনার_ডোমেইন_নাম.crt
    * ইন্টারমিডিয়েট সার্টিফিকেট - DigiCertCA.crt
* প্রতিটি সার্টিফিকেটের শুরু এবং শেষ ট্যাগ অন্তর্ভুক্ত করতে ভুলবেন না। ফলাফলটি এরকম হওয়া উচিত:
```
-----BEGIN CERTIFICATE-----
আপনার প্রাইমারি TLS/SSL সার্টিফিকেট: আপনার_ডোমেইন_নাম.crt
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
আপনার ইন্টারমিডিয়েট সার্টিফিকেট: DigiCertCA.crt
-----END CERTIFICATE-----
```
* সংযুক্ত ফাইলটি আপনার_ডোমেইন_নাম.pem হিসাবে সেভ করুন।
* .pem ফাইলটি এখন ব্যবহারের জন্য প্রস্তুত।

### প্রাইভেট কী এবং সম্পূর্ণ ট্রাস্ট চেইন সহ একটি .pem তৈরি করুন

* আপনার CertCentral অ্যাকাউন্টে, সার্টিফিকেটের অর্ডার বিবরণ পৃষ্ঠায়, আপনার ইন্টারমিডিয়েট (DigiCertCA.crt), রুট (TrustedRoot.crt) এবং প্রাইমারি সার্টিফিকেট (আপনার_ডোমেইন_নাম.crt) ডাউনলোড করুন।
* একটি টেক্সট এডিটর (যেমন নোটপ্যাড) খুলুন এবং নিম্নলিখিত ক্রমে প্রতিটি সার্টিফিকেটের সম্পূর্ণ বডি একই টেক্সট ফাইলে পেস্ট করুন:
    * প্রাইভেট কী - আপনার_ডোমেইন_নাম.key
    * প্রাইমারি সার্টিফিকেট - আপনার_ডোমেইন_নাম.crt
    * ইন্টারমিডিয়েট সার্টিফিকেট - DigiCertCA.crt
    * রুট সার্টিফিকেট - TrustedRoot.crt
* প্রতিটি সার্টিফিকেটের শুরু এবং শেষ ট্যাগ অন্তর্ভুক্ত করতে ভুলবেন না। ফলাফলটি এরকম হওয়া উচিত:
```
-----BEGIN RSA PRIVATE KEY-----
আপনার প্রাইভেট কী: আপনার_ডোমেইন_নাম.key
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
আপনার প্রাইমারি TLS/SSL সার্টিফিকেট: আপনার_ডোমেইন_নাম.crt
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
আপনার ইন্টারমিডিয়েট সার্টিফিকেট: DigiCertCA.crt
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
আপনার রুট সার্টিফিকেট: TrustedRoot.crt
-----END CERTIFICATE-----
```
* সংযুক্ত ফাইলটি আপনার_ডোমেইন_নাম.pem হিসাবে সেভ করুন।
* .pem ফাইলটি এখন ব্যবহারের জন্য প্রস্তুত।
