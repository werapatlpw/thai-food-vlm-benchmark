# Thai Food VLM Benchmark

Ground truth dataset, benchmark results, and evaluation scripts for:

**Benchmarking Vision-Language Models for Thai Food Nutritional Estimation via a Two-Stage Error Analysis Framework**
Werapat Lapawong, Korawat Phonyiam, Piyawan Thongploy, Suporn Pongnumkul
IEEE Access (submitted 2026)

## Contents

- `ground_truth.csv` — Nutritional ground truth for 50 Thai dishes (10 per category × 5 categories), verified by a registered nutritionist. Values are per standard serving, sourced from the INMU Thai Food Composition Database and Thailand's Dietary Guidance for Working-Age Adults (Bureau of Nutrition, Department of Health, Ministry of Public Health).

  | Column | Description |
  |---|---|
  | `dish_id` | Dish number (1–50), matches THFOOD-100 dish selection |
  | `category_en` / `category_th` | One of 5 categories: Noodle/rice-noodle, Curry/soup, Stir-fry, Spicy salad/grilled, Fried/rice |
  | `dish_name_en` / `dish_name_th` | Dish name in English and Thai |
  | `n_ingredients` | Number of ingredients used to compute ground truth |
  | `energy_kcal` | Energy per serving (kcal) |
  | `protein_g` | Protein per serving (g) |
  | `fat_g` | Fat per serving (g) |
  | `carb_g` | Carbohydrate per serving (g) |

- `ground_truth_ingredients.csv` — Ingredient-level breakdown for each of the 50 dishes (493 rows), showing how each dish's ground truth was computed.

  | Column | Description |
  |---|---|
  | `dish_id` | Dish number (1–50), matches `ground_truth.csv` |
  | `dish_name_th` / `category_th` | Dish name and category (Thai) |
  | `ingredient_th` | Ingredient name (Thai) |
  | `baowio_food_code` | Food code in the BaoWio/INMU nutrition database ("–" if hardcoded/USDA-sourced instead) |
  | `baowio_matched_name_th` | Matched database entry name used for nutrition lookup |
  | `energy_kcal_per100g` / `protein_g_per100g` / `fat_g_per100g` / `carb_g_per100g` | Nutrition values per 100g for this ingredient |
  | `portion_g` | Final (corrected) portion size used, in grams |
  | `note` | Free-text measurement/unit reference note, where present |

- `ingredient_nutrition_reference.csv` — Nutrition reference table for the 152 unique ingredients used across all 50 dishes.

  | Column | Description |
  |---|---|
  | `ingredient_th` | Ingredient name (Thai) |
  | `baowio_food_code` | Food code in the BaoWio/INMU nutrition database ("–" if not applicable) |
  | `baowio_matched_name_th` | Matched database entry name |
  | `energy_kcal_per100g` / `protein_g_per100g` / `fat_g_per100g` / `carb_g_per100g` | Nutrition values per 100g |
  | `source_note` | Data source (e.g. `BaoWio`, `USDA <item>`, `HARDCODE`) |

- `benchmark_results_openai.csv`, `benchmark_results_gemini.csv`, `benchmark_results_grok.csv`, `benchmark_results_groq.csv` — Raw per-image predictions (250 rows each = 50 dishes × 5 images) from GPT-4o, Gemini 2.5 Flash, Grok 4.3, and Llama 4 (via Groq) respectively.

  | Column | Description |
  |---|---|
  | `category` | Dish category folder name |
  | `menu_no` | Image-source menu/folder number. **Note:** this is the internal numbering from the raw image folders, not the same as `dish_id` in `ground_truth.csv` — match predictions to ground truth by dish name (`menu_pred` vs. `ชื่อเมนู`/`dish_name_th`), not by number. |
  | `img_no` | Image index within that dish (1–5) |
  | `menu_pred` | Dish name as identified by the model (Thai) |
  | `kcal_pred` / `protein_pred` / `fat_pred` / `carb_pred` | Nutrition values estimated by the model |
  | `raw_response` | Full raw text response from the model |

- `run_benchmark_openai.py`, `run_benchmark_gemini.py`, `run_benchmark_grok.py`, `run_benchmark_groq.py` — Scripts used to query each model's API with the food images and prompt, and save the results above. Each script reads its API credentials from an environment variable (`OPENAI_API_KEY`, `GOOGLE_SERVICE_ACCOUNT_FILE`/`GOOGLE_PROJECT_ID`, `XAI_API_KEY`, `GROQ_API_KEY`) — set these before running. Expects images under `./images/<category>/<menu_no>/<img_no>.jpg`.

- `benchmark_prompt.txt` — Exact zero-shot prompt text used for all models and images (see Appendix A of the paper).

## Images

Source food images are from the THFOOD-100 dataset and are not redistributed here; see the original dataset for image access:

N. Theera-Ampornpunt and P. Treepong, "Thai food recognition using deep learning with cyclical learning rates," *IEEE Access*, vol. 12, pp. 174204–174221, 2024.

## Citation

If you use this dataset or code, please cite:

```
Lapawong, W., Phonyiam, K., Thongploy, P., & Pongnumkul, S. (2026).
Benchmarking Vision-Language Models for Thai Food Nutritional Estimation
via a Two-Stage Error Analysis Framework. IEEE Access.
```
