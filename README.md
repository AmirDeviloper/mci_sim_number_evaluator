# 📱 MCI SIM Card Scraper & Number Evaluator (Python)

This project scrapes sellable SIM card listings from MCI's (Hamrah Aval — Iran's leading mobile operator) online shop and scores each phone number based on a customizable set of "desirability" criteria (repeating digits, sequential digits, mirror patterns, lucky digits, rarity, and more), producing a ranked Excel file of the best available numbers.

The scraper talks directly to MCI's internal shop API (`shop.mci.ir/api/search/v1/products`) rather than parsing HTML, paginates through the full SIM-card catalog, converts listing dates from Gregorian to Jalali (Persian calendar), and batches results into intermediate Excel files as it goes.

The system includes:

* 🌐 **Direct API Scraping** – Paginates through MCI's shop API (up to 625 pages), extracting number, price, listing status, listing date, and category attributes (number pattern, SIM type, permanent/credit) for each SIM card
* 📅 **Jalali Date Conversion** – Converts each listing's creation date from Gregorian to the Persian (Jalali) calendar via `jdatetime`
* 💾 **Batched Excel Export** – Saves results in Excel batches every 50 pages (avoiding data loss on long scraping runs), then merges all batch files into one combined dataset
* 🧮 **Rule-Based Number Scoring** – `PhoneNumberEvaluator` scores each number across 11 weighted criteria: repeating digits, sequential digits, mirror (palindrome) digits, all-even/all-odd digits, double-pairs, "easy to remember" patterns, lucky digits (7/8/9), VIP patterns, grouped numbers, special-date prefixes, and rarity — with special/rare patterns weighted higher
* 🏆 **Ranked Final Output** – Merges scores back into the scraped dataset and exports `simcards_score_final.xlsx`, sorted by total score (highest first)
* 🧩 **Modular Codebase** – Scraping (`mci_scrapper.py`) and scoring (`phone_checker.py`) are cleanly separated for easy reuse or extension

## 📁 Modules

* `mci_scrapper.py` – Scrapes SIM card listings from the MCI shop API, converts dates, batches/merges Excel exports, and drives the end-to-end pipeline
* `phone_checker.py` – `PhoneNumberEvaluator` class implementing the weighted scoring rules used to rank phone numbers

## ▶️ Usage

Run the scraper directly:

```bash
python mci_scrapper.py
```

This will:
1. Scrape all available SIM card listings from MCI's shop into `extracted_files/` (as batched `.xlsx` files)
2. Merge all batch files into a single dataset
3. Score every phone number using `PhoneNumberEvaluator`
4. Save the final ranked result to `simcards_score_final.xlsx`

> ⚠️ **Note:** The current scraped dataset may contain duplicate rows across batches — deduplicate on the phone number column (e.g. `df.drop_duplicates(subset="شماره")`) before further analysis if needed.

⚙️ Requirements
To run this project, you'll need:

* 🐍 Python 3.8 or newer
* 📦 `requests`, `pandas`, `openpyxl`, `jdatetime`


✨ Highlights

* 🌐 Direct API-based scraping (fast and reliable — no HTML parsing fragility)
* 🧮 Fully customizable, weighted number-scoring system
* 💾 Resilient batched export designed for long-running scrapes
* 🧩 Clean separation between data collection and evaluation logic

---

## 📬 Contact

Feel free to reach out if you have questions or feedback!  
Telegram: [@AmirDevil](https://t.me/AmirDevil)

---

## 🛡️ License

This project is licensed under the **MIT License**.  
By contributing, you agree that your contributions will be released under the same license.

---
