# IMD-MBA-Impact: AI for Climate & Work

## Overview
Welcome to **IMD-MBA-Impact**, my portfolio for IMD’s MBA program, showcasing my passion for AI-driven solutions to global challenges. As Maurice McDonald, a veteran and tech enthusiast from Fort Worth, TX, I’ve crafted two projects: **Climate Resilience** (AI to mitigate climate shocks in food supply chains) and **AI Future of Work** (upskilling for an AI economy). Built in RStudio with synthetic `.csv` data and `.png` visualizations, these projects reflect my drive to lead with impact—perfect for IMD’s transformative journey.

---

## Table of Contents
- [Projects](#projects)
- [Files](#files)
- [Code](#code)
- [Setup Instructions](#setup-instructions)
- [Why It Matters](#why-it-matters)
- [Conclusion](#conclusion)
- [License](#license)

---

## Projects

### 1. Climate Resilience: Mitigating Climate Shocks
- **Goal**: Predict crop yields and optimize food supply chains under climate shocks.
- **Focus**: Vietnam (floods) and Kenya (droughts), using AI to forecast rice and maize yields and reduce waste by 15%.
- **Approach**: Linear regression on synthetic climate data, paired with a logistics framework.

### 2. AI Future of Work: Upskilling the Workforce
- **Goal**: Bridge AI skill gaps in developing regions.
- **Focus**: India and Latin America, proposing a 5-year plan to train millions and cut skill gaps by 20%.
- **Approach**: Predictive modeling and visualizations to assess job displacement and training impact.

---

## Files

### `.csv` Files
1. **`Vietnam_Rice_Climate_2023_2025.csv`**:
   - Columns: `Year`, `Precipitation_mm`, `Flood_Affected_Hectares`, `Rice_Yield_MT`
   - Rows: 3 (2023-2025)
   - Description: Simulated rice production data.

2. **`Kenya_Maize_Climate_2023_2025.csv`**:
   - Columns: `Year`, `Precipitation_mm`, `Drought_Severity`, `Maize_Yield_MT`
   - Rows: 3 (2023-2025)
   - Description: Simulated maize production data.

3. **`AI_Upskilling_2025.csv`**:
   - Columns: `Region`, `Jobs_At_Risk`, `AI_Skill_Gap_Percent`, `Workers_Trained`, `Employment_Rate_Post_Training`
   - Rows: 2 (India, Latin America)
   - Description: Synthetic upskilling data.

### `.png` Files (in `/outputs`)
- `climate_shocks_map.png`: Map of floods (Vietnam) and droughts (Kenya).
- `yield_forecast_plot.png`: Rice and maize yield trends (2023-2026).
- `logistics_flowchart.png`: AI-driven supply chain process.
- `skill_gap_heatmap.png`: Skill gaps in India and Latin America.
- `employment_outcomes_plot.png`: Post-training employment rates.

---

## Code

### RStudio Scripts

#### 1. `climate_resilience.R`
```R
library(tmap)
library(ggplot2)
library(caret)
library(sf)

# Load data
vietnam <- read.csv("Vietnam_Rice_Climate_2023_2025.csv")
kenya <- read.csv("Kenya_Maize_Climate_2023_2025.csv")

# Vietnam model
model_vietnam <- train(Rice_Yield_MT ~ Precipitation_mm + Flood_Affected_Hectares, 
                       data = vietnam, method = "lm")
vietnam_2026 <- data.frame(Precipitation_mm = 2000, Flood_Affected_Hectares = 400000)
predict(model_vietnam, vietnam_2026)

# Kenya model
model_kenya <- train(Maize_Yield_MT ~ Precipitation_mm + Drought_Severity, 
                     data = kenya, method = "lm")
kenya_2026 <- data.frame(Precipitation_mm = 500, Drought_Severity = 0.4)
predict(model_kenya, kenya_2026)

# Yield plot
combined <- rbind(cbind(vietnam[, c("Year", "Rice_Yield_MT")], Region = "Vietnam"),
                  cbind(kenya[, c("Year", "Maize_Yield_MT")], Region = "Kenya"))
names(combined) <- c("Year", "Yield_MT", "Region")
plot <- ggplot(combined, aes(x = Year, y = Yield_MT, color = Region)) +
  geom_line() + geom_point() +
  labs(title = "Crop Yield Trends and 2026 Forecast") +
  theme_minimal()
ggsave("outputs/yield_forecast_plot.png", plot, width = 10, height = 6, dpi = 300)

# Map
data <- data.frame(lat = c(10.7769, -1.2921), lon = c(106.7009, 36.8219), 
                   Shock = c("Flood", "Drought"))
points <- st_as_sf(data, coords = c("lon", "lat"), crs = 4326)
tm <- tm_shape(points) + 
  tm_dots(col = "Shock", palette = c("blue", "red"), size = 1) +
  tm_layout(title = "2024-2025 Climate Shocks")
tmap_save(tm, "outputs/climate_shocks_map.png", width = 16, height = 10, dpi = 600)
```

#### 2. `ai_future_work.R`
```R
library(ggplot2)
library(caret)

# Load data
upskill <- read.csv("AI_Upskilling_2025.csv")

# Model
model_skill_gap <- train(AI_Skill_Gap_Percent ~ Workers_Trained + Employment_Rate_Post_Training, 
                         data = upskill, method = "lm")
scenario <- data.frame(Workers_Trained = 1000000, Employment_Rate_Post_Training = 0.75)
predict(model_skill_gap, scenario)

# Heatmap
heatmap <- ggplot(upskill, aes(x = Region, y = 1, fill = AI_Skill_Gap_Percent)) +
  geom_tile() + scale_fill_gradient(low = "lightgreen", high = "darkred") +
  labs(title = "AI Skill Gaps (2025)") +
  theme_minimal()
ggsave("outputs/skill_gap_heatmap.png", heatmap, width = 10, height = 6, dpi = 300)

# Bar plot
bar <- ggplot(upskill, aes(x = Region, y = Employment_Rate_Post_Training, fill = Region)) +
  geom_bar(stat = "identity", width = 0.5) +
  labs(title = "Employment Rates Post-AI Upskilling (2025)", y = "Employment Rate") +
  theme_minimal()
ggsave("outputs/employment_outcomes_plot.png", bar, width = 10, height = 6, dpi = 300)
```

#### 3. `logistics_flowchart.R`
```R
library(DiagrammeR)
grViz("
digraph {
  graph [rankdir = LR]
  node [shape = box, style = filled, fillcolor = lightblue]
  A [label = 'Climate Data Collection']
  B [label = 'AI Predictive Modeling']
  C [label = 'Logistics Optimization']
  D [label = 'Equitable Distribution']
  E [label = 'Food Security']
  A -> B -> C -> D -> E
}")
export_svg(.) %>% writeLines("outputs/logistics_flowchart.svg")
# Convert SVG to PNG manually or use webshot if preferred
```

---

## Setup Instructions
1. **Clone the Repo**:
   ```bash
   git clone https://github.com/emcdo411/IMD-MBA-Impact.git
   cd IMD-MBA-Impact
   ```
2. **Install R Packages**:
   ```R
   install.packages(c("tmap", "ggplot2", "caret", "sf", "DiagrammeR"))
   ```
3. **Run Scripts**:
   - Open each `.R` file in RStudio.
   - Source them (`Ctrl+Shift+S`) to generate `.png` outputs in `/outputs`.

**Note**: `tmap` requires a map provider (e.g., OpenStreetMap); adjust `climate_resilience.R` if needed. `logistics_flowchart.R` outputs SVG—convert to PNG manually or add `webshot`.

---

## Why It Matters
Why IMD? These projects aren’t just code—they’re my blueprint for impact. **Climate Resilience** tackles food security head-on, showing how AI can turn data into survival for farmers in Vietnam and Kenya—leadership under pressure, IMD-style. **AI Future of Work** fights inequality, upskilling millions to thrive in an AI world—a vision I’ll scale with IMD’s global lens. As a veteran, I’ve led through chaos; now, I’m ready to lead with purpose. IMD’s real-world focus and diverse cohort will sharpen my skills to bridge tech and humanity—because the future demands leaders who don’t just analyze, but act.

---

## Conclusion
**IMD-MBA-Impact** is my case for IMD: technical depth, social purpose, and leadership potential. From climate shocks to skill gaps, I’m tackling tomorrow’s challenges today. I’m eager to bring this energy to Lausanne, learn from IMD’s community, and launch a career driving equitable, AI-powered change. Let’s make it happen!

---

## License
MIT License—open for collaboration, mirroring my IMD goals. See `LICENSE`.

---

### Suggested Repo Name
**`IMD-MBA-Impact`**  
- Why: Highlights your IMD MBA ambition and focus on impactful AI solutions.

### Notes
- **Projects**: Combines Climate Resilience and AI Future of Work, aligned with your IMD docs.
- **Files**: Includes all `.csv` and `.png` files from your reports—update with your exact paths if different (e.g., `C:/Users/Veteran/`).
- **Code**: Simplified R scripts from your methodology; assumes basic data—adjust to your specifics (e.g., add 2026 forecasts manually).
- **Why It Matters**: Ties your veteran experience and project goals to IMD’s leadership ethos.

Ready to upload to `https://github.com/emcdo411/IMD-MBA-Impact`? Share tweaks or your exact file names if I’ve missed anything! 😊 What’s next?
