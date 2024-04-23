
# AI4ALL Recipe Recommendation System

## Project Description
As global food demand rises, the environmental impact associated with it intensifies. Notably, about a third of all human-caused greenhouse gas emissions are linked to food, primarily from agricultural practices and land use. Our Recipe Recommendation System is designed to empower eco-conscious consumers, educators, and community organizations by providing smart, machine learning-driven tools to make food choices that are both healthy for people and the planet. By optimizing recipe selections based on personal dietary preferences and environmental impact, our system aims to reduce the user's carbon footprint through informed decisions.

## Project Details
- **Goal:** Leverage advanced machine learning algorithms to recommend recipes that minimize carbon footprints.
- **Product:** A recipe recommendation engine that filters and suggests recipes based on users' historical ratings and other relevant features.
- **Target Users:** Eco-conscious individuals, vegans, vegetarians, educators, students, nonprofits, and community organizations.
- **Approaches Used:**
  - **Collaborative Filtering:** Predicts and recommends new recipes using user ratings and similarities among users.
  - **Content Filtering:** Matches recipes to users based on carbon footprint, preparation difficulty, and nutritional content.
  - **Singular Value Decomposition (SVD):** Analyzes ratings data to capture hidden preferences and attributes, improving recommendation accuracy.

## Installation and Usage Instructions
To set up the Recipe Recommendation System on your local machine, follow these steps:
1. Clone the repository:
   ```bash
   git clone https://github.com/alimaazamat/AI4ALL.git
2. Nagivate to project directory:
   ```bash
   cd AI4ALL
3. Install the required dependencies, such as Python Extension and Jupyter Extension
4. Install the required libraries
   ```bash
   pip install scikit-surprise pandas numpy matplotlib seaborn scipy scikit-learn
5. Open `.ipynb` file by opening AI4ALL folder in code editor
6. Launch Jupyter Notebook
   ```bash
   jupyter notebook
7. Run the Notebook by clicking "Execute Cell" on left side (looks like a play button)

## Code Examples and API Documentation
Below are some code snippets:

- **Calculating User Similarity:**
  ```python
  from scipy.sparse import csr_matrix
  from sklearn.metrics.pairwise import cosine_similarity
  
  # Create a CSR matrix
  sparse_user_item_matrix = csr_matrix((ratings_df['rating'], (ratings_df['userID'], ratings_df['itemID'])))
  
  # Compute cosine similarity
  cosine_sim = cosine_similarity(sparse_user_item_matrix, dense_output=False)
  
  # Convert cosine_sim to a DataFrame
  user_similarity_df = pd.DataFrame(cosine_sim.toarray(), index=user_ids, columns=user_ids)
  print(user_similarity_df.head())

- **Generating Recommendations:**
  ```python
  def get_recommendations(user_similarity_df, ratings_df, raw_recipes_df, user_id, top_n=10):
    """Get top item recommendations for a given user based on user similarity."""
  
    # Map itemIDs to recipe names
    recipe_id_name_dict = pd.Series(raw_recipes_df.name.values, index=raw_recipes_df.id).to_dict()
  
    # Get top similar users
    similar_users = get_similar_users(user_similarity_df, user_id, top_n)
  
    # Filter ratings to only include ratings from similar users
    similar_users_ratings = ratings_df[ratings_df['userID'].isin(similar_users)]
  
    # Calculate mean rating for each item by similar users
    item_ratings = similar_users_ratings.groupby('itemID')['rating'].mean().reset_index()
  
    # Get the items the user hasn't rated yet
    user_rated_items = ratings_df[ratings_df['userID'] == user_id]['itemID']
    temp = item_ratings[~item_ratings['itemID'].isin(user_rated_items)].sort_values('rating', ascending=False)

    #filter top 10 based on existent ID's and filtering nonexistent ones
    recommendations = pd.DataFrame(columns = temp.columns.tolist())
    for index, row in temp.iterrows():
      if row['itemID'] in recipe_id_name_dict:
        recommendations.loc[len(recommendations.index)] = row
      if len(recommendations) >= 10:
        break

    # Append recipe names to the recommendations
    recommendations['recipeName'] = recommendations['itemID'].apply(lambda x: recipe_id_name_dict.get(x, f"Recipe ID: {x}"))
    return recommendations[['itemID', 'recipeName', 'rating']]

## Contribution Guidelines
Interested in contributing? Here’s how you can help:
1. Fork the repository.
2. Create your feature branch (git checkout -b feature/AmazingFeature).
   ```bash
   git checkout -b feature/AmazingFeature
3. Commit your changes.
   ```bash
   git commit -m 'Add some AmazingFeature'
4. Push to the branch.
   ```bash
   git push origin feature/AmazingFeature
5. Open a Pull Request.

## Acknowledgments and References
Thanks to our Instructor Rebecca Funke and our Student Coordinator for teaching us the principles of AI and guiding us through this project. And thanks to AI4All for creating this organization that allows us to create a group and organzie this project. 

**References**
- Alkerwi, A., Vernier, C., Sauvageot, N., Crichton, G. E., & Elias, M. F. (2015). *Demographic and socioeconomic disparity in nutrition: application of a novel Correlated Component Regression approach*. BMJ Open, 5(5), e006814. [https://doi.org/10.1136/bmjopen-2014-006814](https://doi.org/10.1136/bmjopen-2014-006814)

- Center for Sustainable Systems. (2023, August). *Carbon Footprint Factsheet*. Retrieved from [https://css.umich.edu/publications/factsheets/sustainability-indicators/carbon-footprint-factsheet](https://css.umich.edu/publications/factsheets/sustainability-indicators/carbon-footprint-factsheet)

- Khan, I. (2021, January 17). *Classification & Regression in Machine Learning*. Medium. Available at [https://medium.com/unpackai/classification-regression-in-machine-learning-7cf3b13b0b09](https://medium.com/unpackai/classification-regression-in-machine-learning-7cf3b13b0b09)

- PhD, Mary Reagan. (2021, April 2). *Understanding Bias and Fairness in AI Systems*. Medium. Available at [https://towardsdatascience.com/understanding-bias-and-fairness-in-ai-systems-6f7fbfe267f3](https://towardsdatascience.com/understanding-bias-and-fairness-in-ai-systems-6f7fbfe267f3)

- Ritchie, H. (2024, March 18). *You want to reduce the carbon footprint of your food? Focus on what you eat, not whether your food is local*. Our World in Data. [https://ourworldindata.org/food-choice-vs-eating-local](https://ourworldindata.org/food-choice-vs-eating-local)

- Ritchie, H., Rosado, P., & Roser, M. (2022, December 2). *Environmental impacts of food production*. Our World in Data. [https://ourworldindata.org/environmental-impacts-of-food](https://ourworldindata.org/environmental-impacts-of-food)

- U.S. Department of Health and Human Services. (2023, April). *Food Accessibility, Insecurity and Health Outcomes*. National Institute of Minority Health and Health Disparities. [https://www.nimhd.nih.gov/resources/understanding-health-disparities/food-accessibility-insecurity-and-health-outcomes.html](https://www.nimhd.nih.gov/resources/understanding-health-disparities/food-accessibility-insecurity-and-health-outcomes.html)

- United Nations. (n.d.). *Food and climate change: Healthy diets for a healthier planet*. United Nations. [https://www.un.org/en/climatechange/science/climate-issues/food](https://www.un.org/en/climatechange/science/climate-issues/food)

