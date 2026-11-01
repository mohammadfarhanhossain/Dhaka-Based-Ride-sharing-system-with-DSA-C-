# SOHOJJATRA (সহযাত্র) - DSA Concepts & Architecture Documentation

এই নথিতে **SOHOJJATRA Ride-Sharing System** প্রজেক্টে ব্যবহৃত সকল Data Structures এবং Algorithms (DSA)-এর বিস্তারিত বিবরণ দেওয়া হলো—কোন কনসেপ্টটি কোথায় ব্যবহার করা হয়েছে, কেন ব্যবহার করা হয়েছে এবং এর টাইম ও স্পেস কমপ্লেক্সিটি কত।

---

## ১. Graph (Adjacency List with Doubly Linked List)

* **ফাইল:** [`graph.hpp`](file:///d:/Downloads/dsa2/graph.hpp), [`graph.cpp`](file:///d:/Downloads/dsa2/graph.cpp)
* **কোথায় ব্যবহৃত:** ঢাকা শহরের রোড নেটওয়ার্ক তৈরি, রাস্তা যোগ/বিয়োগ এবং লোকেশন সংযোগ ব্যবস্থাপনায়।
* **কেন ব্যবহার করা হয়েছে:**
  - শহর এবং তাদের মধ্যবর্তী রাস্তাসমূহ একটি **Weighted Undirected Graph**।
  - শহরগুলোর নাম ডাইনামিক `Vector<string> cities` এ সংরক্ষিত থাকে।
  - প্রতিটি শহরের সংলগ্ন রাস্তাসমূহ (Adjacency List) হ্যান্ডেল করতে কাস্টম **Doubly Linked List** (`EdgeNode`) ব্যবহার করা হয়েছে।
  - **সুবিধা:** কোনো রাস্তা বাতিল (`removeRoad`) বা শহর মুছে ফেলার সময় Doubly Linked List-এর `prev` এবং `next` পয়েন্টার থাকায় $O(1)$ টাইমে মেমোরি থেকে এজ আনলিঙ্ক ও ডিলিট করা যায়।
* **কমপ্লেক্সিটি:**
  - রাস্তা যোগ করা (`addRoad`): $O(1)$
  - রাস্তা মোছা (`removeRoad`): $O(E_{node})$ (ডিগ্রির সমান)
  - স্পেস কমপ্লেক্সিটি: $O(V + E)$

---

## ২. BFS (Breadth-First Search) Algorithm with FIFO Queue

* **ফাইল:** [`graph.cpp`](file:///d:/Downloads/dsa2/graph.cpp) (`bfsPath`), [`queue.hpp`](file:///d:/Downloads/dsa2/queue.hpp)
* **কোথায় ব্যবহৃত:**
  1. যাত্রী যখন রাইড বুক করেন, পিকআপ থেকে ডেস্টিনেশনের **সর্বোত্তম ও শর্টেস্ট পাথ** বের করতে।
  2. ড্রাইভার পাওয়ার পর ড্রাইভারের বর্তমান অবস্থান থেকে যাত্রীর পিকআপ পয়েন্টে পৌঁছানোর শর্টেস্ট পাথ ও দূরত্ব মাপতে।
* **কেন ব্যবহার করা হয়েছে:**
  - আনওয়েটেড বা হপ-ভিত্তিক শর্টেস্ট রুট বের করার জন্য BFS আদর্শ।
  - ট্রাভার্সালের জন্য কাস্টম জেনেরিক **Queue (FIFO)** ব্যবহার করা হয়েছে।
  - `parent[]` অ্যারে ব্যাকট্র্যাকিং করে সম্পূর্ণ রুট (`tempPath`) নিখুঁতভাবে তৈরি করা হয়।
* **কমপ্লেক্সিটি:**
  - টাইম কমপ্লেক্সিটি: $O(V + E)$
  - স্পেস কমপ্লেক্সিটি: $O(V)$ (`visited`, `parent`, `queue`-এর জন্য)

---

## ৩. DFS (Depth-First Search) Algorithm (Recursion & Call Stack)

* **ফাইল:** [`graph.cpp`](file:///d:/Downloads/dsa2/graph.cpp) (`dfsPath`, `dfsHelper`)
* **কোথায় ব্যবহৃত:**
  - পিকআপ এবং ডেস্টিনেশনের মধ্যে সরাসরি বা ঘুরে যাওয়ার কোনো পথ আদৌ বিদ্যমান কি না (Reachability Check) এবং বিকল্প রুট (Alternative Route) জেনারেট করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - রিকার্শনের মাধ্যমে সিস্টেম কল স্ট্যাক ব্যবহার করে গভীরের নোড ট্রাভার্স করা হয়।
  - ব্যাকট্র্যাকিংয়ের মাধ্যমে যদি ডেস্টিনেশনে না পৌঁছায়, তবে পাথ থেকে নোড `pop_back()` করে অন্য শাখা অনুসন্ধান করা হয়।
* **কমপ্লেক্সিটি:**
  - টাইম কমপ্লেক্সিটি: $O(V + E)$
  - স্পেস কমপ্লেক্সিটি: $O(V)$ (Recursion Call Stack)

---

## ৪. Min-Heap (Priority Queue) for Driver Matching

* **ফাইল:** [`heap.hpp`](file:///d:/Downloads/dsa2/heap.hpp), [`heap.cpp`](file:///d:/Downloads/dsa2/heap.cpp), [`main.cpp`](file:///d:/Downloads/dsa2/main.cpp) (`matchDriver`)
* **কোথায় ব্যবহৃত:**
  - রাইডার যখন নির্দিষ্ট কোনো ভেহিকেল (Bike/CNG/UberX) পছন্দ করেন, তখন সিস্টেমের সব ফ্রি ড্রাইভারের মধ্য থেকে **নিকটতম এবং সেরা ড্রাইভার** খুঁজে পেতে।
* **কেন ব্যবহার করা হয়েছে:**
  - সাধারণ লিনিয়ার সার্চের বদলে Min-Heap ব্যবহার করলে সর্বনিম্ন দূরত্বের ড্রাইভার সবার উপরে (রুট নোডে) থাকে।
  - **Priority Tie-Breaker Logic:** যদি দুজন ড্রাইভারের দূরত্ব সমান হয়, তবে যার রেটিং বেশি (`rating > smallest.rating`) সে প্রায়োরিটি পাবে।
* **কমপ্লেক্সিটি:**
  - ইনসার্শন (`push`): $O(\log K)$
  - টপ ড্রাইভার এক্সট্র্যাক্ট (`extractMin`): $O(\log K)$
  - $K$ জন ড্রাইভারের জন্য ম্যাচিং: $O(K \log K)$

---

## ৫. Stack (LIFO: Last-In, First-Out) for Ride History

* **ফাইল:** [`stack.hpp`](file:///d:/Downloads/dsa2/stack.hpp), [`stack.cpp`](file:///d:/Downloads/dsa2/stack.cpp) (`displayTripHistory`)
* **কোথায় ব্যবহৃত:**
  - যাত্রী ও ড্রাইভারদের অতীত ট্রিপ হিস্ট্রি প্রদর্শনে।
* **কেন ব্যবহার করা হয়েছে:**
  - রিয়েল-ওয়ার্ল্ড অ্যাপে ইউজাররা সবসময় তাদের **সবচেয়ে সাম্প্রতিক (Most Recent) ট্রিপ** সবার উপরে দেখতে চান।
  - ফাইল থেকে রিড করার পর ট্রিপগুলোকে স্ট্যাকে `push` করা হয় এবং ডিসপ্লে করার সময় `pop` করা হয়। ফলে স্বয়ংক্রিয়ভাবে LIFO ক্রমে সাম্প্রতিকতম রাইড আগে প্রদর্শিত হয়।
* **কমপ্লেক্সিটি:**
  - পুশ / পপ: $O(1)$
  - সম্পূর্ণ হিস্ট্রি প্রদর্শন: $O(N)$

---

## ৬. Singly Linked List for Favorite Locations

* **ফাইল:** [`linkedlist.hpp`](file:///d:/Downloads/dsa2/linkedlist.hpp), [`rider.hpp`](file:///d:/Downloads/dsa2/rider.hpp) (`favorites`)
* **কোথায় ব্যবহৃত:**
  - প্রতিটি রাইডারের প্রোফাইলে তাদের পছন্দের স্থানসমূহ (Favorite Locations - যেমন Home, Office) সংরক্ষণ ও পরিচালনায়।
* **কেন ব্যবহার করা হয়েছে:**
  - পছন্দের স্থান যেকোনো সময় যোগ (`insertAtTail`) বা মাঝখান থেকে মুছে (`deleteNode`) ফেলা হতে পারে।
  - অ্যারে শিফটিং পরিহার করে পয়েন্টার রিলিংকিংয়ের মাধ্যমে ডাইনামিকালি মেমোরি নিয়ন্ত্রণ করা যায়।
* **কমপ্লেক্সিটি:**
  - শেষে ইনসার্ট: $O(1)$ (টেইল পয়েন্টার থাকায়)
  - নির্দিষ্ট ফেভারিট ডিলিট: $O(N)$
  - স্পেস: $O(N)$

---

## ৭. FIFO Queue for Wallet Top-Up Requests

* **ফাইল:** [`queue.hpp`](file:///d:/Downloads/dsa2/queue.hpp), [`payment.hpp`](file:///d:/Downloads/dsa2/payment.hpp) (`pendingQueue`)
* **কোথায় ব্যবহৃত:**
  - যাত্রীরা যখন ওয়ালেটে টাকা রিচার্জের অনুরোধ পাঠান, সেই পেন্ডিং রিকোয়েস্টগুলো ক্রমানুসারে সংরক্ষণ করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - ফেয়ারনেস নীতি: **First-Come, First-Served (FCFS)**। যে যাত্রী আগে টাকা যোগ করার রিকোয়েস্ট দিয়েছেন, অ্যাডমিন প্যানেল থেকে তার রিকোয়েস্টই আগে অনুমোদিত (`approveFirstRequest`) হবে।
* **কমপ্লেক্সিটি:**
  - এনকিউ (`enqueue`): $O(1)$
  - ডিকিউ (`dequeue`): $O(1)$

---

## ৮. Binary Search Tree (BST) for User ID Lookup

* **ফাইল:** [`bst.hpp`](file:///d:/Downloads/dsa2/bst.hpp), [`admin.hpp`](file:///d:/Downloads/dsa2/admin.hpp), [`admin.cpp`](file:///d:/Downloads/dsa2/admin.cpp)
* **কোথায় ব্যবহৃত:**
  - অ্যাডমিন প্যানেলে ইউজার আইডি দিয়ে তৎক্ষণাৎ ইউজার প্রোফাইল অনুসন্ধান করতে (`searchUserByBST`)।
* **কেন ব্যবহার করা হয়েছে:**
  - হাজার হাজার ইউজারের মধ্য থেকে আইডি ধরে $O(N)$ লিনিয়ার সার্চ না করে $O(\log N)$ এভারেজ টাইমে খোঁজার জন্য BST ব্যবহার করা হয়েছে।
* **কমপ্লেক্সিটি:**
  - সার্চ / ইনসার্ট: Average $O(\log N)$, Worst-case $O(N)$
  - স্পেস কমপ্লেক্সিটি: $O(N)$

---

## ৯. Recursive Binary Search (Divide & Conquer)

* **ফাইল:** [`sorting.hpp`](file:///d:/Downloads/dsa2/sorting.hpp) (`binarySearchRecursive`), [`driver.cpp`](file:///d:/Downloads/dsa2/driver.cpp)
* **কোথায় ব্যবহৃত:**
  - ড্রাইভার আইডি দিয়ে নির্দিষ্ট ড্রাইভারকে দ্রুত খুঁজে বের করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - ড্রাইভারদের আইডি স্বয়ংক্রিয়ভাবে সর্টেড অর্ডারে থাকে ($1, 2, 3...$)।
  - ডিভাইড অ্যান্ড কনকোয়ার নীতির সাহায্যে অ্যারের মধ্যবিন্দু নির্ণয় করে রিকার্সিভলি সার্চ স্পেস প্রতি ধাপে অর্ধেক করে ফেলা হয়।
* **কমপ্লেক্সিটি:**
  - টাইম কমপ্লেক্সিটি: $O(\log N)$
  - স্পেস কমপ্লেক্সিটি: $O(\log N)$ (রিকার্শন কল স্ট্যাক)

---

## ১০. Merge Sort for Driver Leaderboard

* **ফাইল:** [`sorting.hpp`](file:///d:/Downloads/dsa2/sorting.hpp), [`sorting.cpp`](file:///d:/Downloads/dsa2/sorting.cpp) (`mergeSortDriverLeaderboard`)
* **কোথায় ব্যবহৃত:**
  - অ্যাডমিন প্যানেলে সেরা ড্রাইভারদের লিডারবোর্ড র‍্যাঙ্কিং তৈরি করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - **গ্যারান্টিযুক্ত পারফরম্যান্স ও স্ট্যাবিলিটি:** কুইক সর্টের মতো ওর্স্ট-কেস $O(N^2)$ ঝুঁকি এতে নেই, সবসময় $O(N \log N)$ নিশ্চিত করে।
  - **মাল্টি-ক্রাইটেরিয়া সর্টিং (`isBetterDriver`):** প্রথমে অ্যাভারেজ রেটিং দিয়ে তুলনা করা হয়; রেটিং সমান হলে মোট ট্রিপ কাউন্ট দিয়ে টাই-ব্রেক করা হয়।
* **কমপ্লেক্সিটি:**
  - টাইম কমপ্লেক্সিটি: $O(N \log N)$ (সবক্ষেত্রে)
  - অক্সিলারি স্পেস: $O(N)$

---

## ১১. Selection Sort for Demand Heatmap Analysis

* **ফাইল:** [`sorting.hpp`](file:///d:/Downloads/dsa2/sorting.hpp), [`sorting.cpp`](file:///d:/Downloads/dsa2/sorting.cpp) (`selectionSortDemandDesc`)
* **কোথায় ব্যবহৃত:**
  - ঢাকা শহরের কোন কোন এলাকায় সবচেয়ে বেশি রাইডের চাহিদা রয়েছে তা বড় থেকে ছোট ক্রমে অ্যানালাইসিস করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - শহরের সংখ্যা তুলনামূলকভাবে সীমাবদ্ধ (স্মল সাইজ ডেটাসেট)।
  - সিলেকশন সর্ট প্রতি পাসে সর্বোচ্চ ডিমান্ডের লোকেশনটি খুঁজে বের করে সরাসরি স্বয়ংক্রিয়ভাবে শুরুতে স্থান দেয় (Minimum Memory Swaps)।
* **কমপ্লেক্সিটি:**
  - টাইম কমপ্লেক্সিটি: $O(V^2)$
  - স্পেস কমপ্লেক্সিটি: $O(1)$ ইন-প্লেস

---

## ১২. Dynamic Array (Custom Template Vector)

* **ফাইল:** [`vector.hpp`](file:///d:/Downloads/dsa2/vector.hpp)
* **কোথায় ব্যবহৃত:**
  - পুরো প্রজেক্ট জুড়ে `Vector<string>`, `Vector<int>`, `Vector<DriverInfo>`, `Vector<RiderInfo>` ইত্যাদি স্টোর ও ম্যানিপুলেট করতে।
* **কেন ব্যবহার করা হয়েছে:**
  - কোনো STL `std::vector` ব্যবহার না করে র-পয়েন্টার অ্যালোকেশন (`new[]`/`delete[]`), অ্যামরটাইজড ডাবলিং ক্যাপাসিটি রিসাইজিং এবং বাউন্ড চেকসহ স্বয়ংসম্পূর্ণ ভেক্টর ক্লাস তৈরি করা হয়েছে।
* **কমপ্লেক্সিটি:**
  - ইনডেক্সিং (`operator[]`): $O(1)$
  - পুশ ব্যাক (`push_back`): Amortized $O(1)$
  - ইরেজ (`erase`): $O(N)$

---

## এক নজরে সামারি টেবিল (Viva Cheat-Sheet)

| DSA Concept | ফাইল | প্রধান ব্যবহার | Time Complexity |
|---|---|---|---|
| **Graph (Adjacency DLL)** | `graph.hpp`, `graph.cpp` | ঢাকা শহরের রোড নেটওয়ার্ক | $O(1)$ Add, $O(V+E)$ Traversal |
| **BFS Algorithm** | `graph.cpp` | সর্বোত্তম শর্টেস্ট রুট ও দূরত্ব | $O(V + E)$ |
| **DFS Algorithm** | `graph.cpp` | রিচেবিলিটি ও অল্টারনেটিভ রুট | $O(V + E)$ |
| **Min-Heap** | `heap.hpp`, `heap.cpp` | নিকটতম ও বেস্ট রেটেড ড্রাইভার ম্যাচিং | $O(\log K)$ |
| **Stack (LIFO)** | `stack.hpp`, `stack.cpp` | সাম্প্রতিকতম ট্রিপ হিস্ট্রি প্রদর্শন | $O(1)$ Push/Pop |
| **Queue (FIFO)** | `queue.hpp` | BFS ট্রাভার্সাল ও ওয়ালেট রিকোয়েস্ট কিউ | $O(1)$ Enqueue/Dequeue |
| **Singly Linked List** | `linkedlist.hpp` | রাইডারের ফেভারিট লোকেশন লিস্ট | $O(1)$ Insert Tail, $O(N)$ Delete |
| **Binary Search Tree** | `bst.hpp` | অ্যাডমিনের দ্রুত ইউজার আইডি অনুসন্ধান | $O(\log N)$ Average |
| **Binary Search (Rec)** | `sorting.hpp`, `driver.cpp` | ড্রাইভার আইডি দিয়ে ইনস্ট্যান্ট লুকআপ | $O(\log N)$ |
| **Merge Sort** | `sorting.cpp` | ড্রাইভারদের রেটিং ও ট্রিপ লিডারবোর্ড | $O(N \log N)$ |
| **Selection Sort** | `sorting.cpp` | এলাকাভিত্তিক ট্রিপ ডিমান্ড হিটম্যাপ | $O(V^2)$ |
| **Dynamic Vector** | `vector.hpp` | কাস্টম ডায়নামিক অ্যারে মেমোরি ম্যানেজমেন্ট | $O(1)$ Indexing |
