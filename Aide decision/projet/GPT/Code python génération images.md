---
tags:
  - aideDecision
---
```python
import random

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

from PIL import Image, ImageDraw, ImageFont

import os

import shutil

  

# Configuration de Seaborn pour des graphismes esthétiques

sns.set(style="whitegrid", palette="pastel")

  

# ================================

# 1. Préparation de l'Environnement

# ================================

  

# Création d'un dossier pour sauvegarder les images séparées

output_dir = "output_images"

if os.path.exists(output_dir):

shutil.rmtree(output_dir)

os.makedirs(output_dir)

  

# Fonction pour obtenir une police compatible

def get_font(font_size=14):

try:

return ImageFont.truetype("arial.ttf", font_size)

except IOError:

# Utiliser une police par défaut si Arial n'est pas disponible

return ImageFont.load_default()

  

# ================================

# 2. Définition des Élèves et des Écoles

# ================================

  

# Liste de 20 élèves nommés

students = [

'Alice', 'Bob', 'Charlie', 'Diana', 'Ethan', 'Fiona', 'George', 'Hannah',

'Ian', 'Julia', 'Kevin', 'Laura', 'Michael', 'Nina', 'Oscar', 'Paula',

'Quentin', 'Rachel', 'Steve', 'Tina'

]

  

# Liste de 5 écoles avec leurs capacités

schools = {

'École A': 4,

'École B': 4,

'École C': 4,

'École D': 4,

'École E': 4

}

  

# ================================

# 3. Attribution des Préférences et Priorités

# ================================

  

# Fonction pour générer des préférences pondérées avec randomisation

def generate_weighted_student_preferences(students, schools, randomness=0.3):

"""

Génère des préférences des élèves basées sur la popularité des écoles avec une randomisation contrôlée.

:param students: Liste des élèves

:param schools: Dictionnaire des écoles avec leurs capacités

:param randomness: Proportion de randomisation (entre 0 et 1)

:return: Dictionnaire des préférences des élèves

"""

# Définir la popularité des écoles (plus une école est populaire, plus elle est susceptible d'être en haut des préférences)

school_popularity = {

'École A': 5, # Très populaire

'École B': 4, # Populaire

'École C': 3, # Moyennement populaire

'École D': 2, # Moins populaire

'École E': 1 # Peu populaire

}

  

# Calculer le total de popularité pour normaliser les poids

total_popularity = sum(school_popularity.values())

  

# Générer les préférences

student_prefs = {}

for student in students:

prefs = []

available_schools = list(schools.keys())

while available_schools:

# Calculer les poids avec la randomisation

weights = []

for school in available_schools:

weight = school_popularity[school] * (1 - randomness) + (randomness / len(available_schools))

weights.append(weight)

# Normaliser les poids

weights = [w / sum(weights) for w in weights]

# Choisir la prochaine école basée sur les poids

chosen_school = random.choices(available_schools, weights=weights, k=1)[0]

prefs.append(chosen_school)

available_schools.remove(chosen_school)

student_prefs[student] = prefs

return student_prefs

  

# Fonction pour générer des priorités pondérées avec randomisation

def generate_weighted_school_priorities(schools, students, randomness=0.3):

"""

Génère des priorités des écoles basées sur un critère défini avec une randomisation contrôlée.

:param schools: Dictionnaire des écoles avec leurs capacités

:param students: Liste des élèves

:param randomness: Proportion de randomisation (entre 0 et 1)

:return: Dictionnaire des priorités des écoles

"""

# Simuler que les élèves ayant des noms plus tôt dans l'alphabet sont mieux classés

sorted_students = sorted(students)

school_priorities = {}

for school in schools:

priorities = []

available_students = sorted_students.copy()

while available_students:

# Calculer les poids avec la randomisation

weights = []

for student in available_students:

# Attribuer un poids basé sur l'ordre alphabétique

weight = (len(sorted_students) - sorted_students.index(student)) * (1 - randomness) + (randomness / len(available_students))

weights.append(weight)

# Normaliser les poids

weights = [w / sum(weights) for w in weights]

# Choisir le prochain élève basé sur les poids

chosen_student = random.choices(available_students, weights=weights, k=1)[0]

priorities.append(chosen_student)

available_students.remove(chosen_student)

school_priorities[school] = priorities

return school_priorities

  

# Génération des préférences et priorités pondérées avec 30% de randomisation

student_prefs = generate_weighted_student_preferences(students, schools, randomness=0.3)

school_priorities = generate_weighted_school_priorities(schools, students, randomness=0.3)

  

# ================================

# 4. Algorithme de Boston

# ================================

  

def boston_algorithm(student_prefs, school_priorities, schools):

assignments = {school: [] for school in schools}

unassigned_students = set(students)

for rank in range(len(schools)):

if not unassigned_students:

break

# Les élèves non assignés proposent à leur k-ième choix

proposals = {}

for student in list(unassigned_students):

if rank < len(student_prefs[student]):

preferred_school = student_prefs[student][rank]

proposals.setdefault(preferred_school, []).append(student)

# Les écoles acceptent les élèves en fonction de leurs priorités

for school, applicants in proposals.items():

if len(assignments[school]) < schools[school]:

# Priorité des écoles

priority = school_priorities[school]

for applicant in priority:

if applicant in applicants:

if len(assignments[school]) < schools[school]:

assignments[school].append(applicant)

unassigned_students.remove(applicant)

if len(assignments[school]) >= schools[school]:

break

rank += 1

return assignments

  

# Exécution de l'algorithme de Boston

final_assignment_boston = boston_algorithm(student_prefs, school_priorities, schools)

  

# ================================

# 5. Algorithme d'Acceptation Différée (Gale-Shapley)

# ================================

  

def deferred_acceptance_algorithm(student_prefs, school_priorities, schools):

# Initialisation

free_students = list(students)

proposals = {student: 0 for student in students} # Nombre de propositions effectuées

assignments = {school: [] for school in schools}

while free_students:

student = free_students.pop(0)

if proposals[student] >= len(student_prefs[student]):

continue # L'élève a épuisé toutes ses options

# L'élève propose à son choix suivant

school = student_prefs[student][proposals[student]]

proposals[student] += 1

assignments[school].append(student)

# Trier les élèves assignés selon la priorité de l'école

assignments[school].sort(key=lambda x: school_priorities[school].index(x))

# Si l'école dépasse sa capacité, rejeter le dernier élève

if len(assignments[school]) > schools[school]:

rejected_student = assignments[school].pop(-1)

free_students.append(rejected_student)

# Créer un dictionnaire inversé pour les assignations

final_assignments = {}

for school, assigned_students in assignments.items():

for student in assigned_students:

final_assignments[student] = school

return final_assignments

  

# Exécution de l'algorithme d'Acceptation Différée

final_assignment_da = deferred_acceptance_algorithm(student_prefs, school_priorities, schools)

  

# ================================

# 6. Visualisations

# ================================

  

# 6.1 Satisfaction des Élèves (Algorithme de Boston)

def plot_student_satisfaction_boston(assignments, student_prefs, output_path):

satisfaction = {'Premier Choix': 0, 'Deuxième Choix': 0, 'Troisième Choix': 0, 'Autres': 0}

for school, assigned_students in assignments.items():

for student in assigned_students:

rank = student_prefs[student].index(school) + 1

if rank == 1:

satisfaction['Premier Choix'] += 1

elif rank == 2:

satisfaction['Deuxième Choix'] += 1

elif rank == 3:

satisfaction['Troisième Choix'] += 1

else:

satisfaction['Autres'] += 1

labels = list(satisfaction.keys())

values = list(satisfaction.values())

plt.figure(figsize=(10,6))

sns.barplot(x=labels, y=values, palette="Blues_d")

plt.title('Satisfaction des Élèves (Algorithme de Boston)', fontsize=18)

plt.ylabel('Nombre d\'Élèves', fontsize=14)

plt.xlabel('Rang de Préférence', fontsize=14)

plt.xticks(fontsize=12)

plt.yticks(fontsize=12)

plt.tight_layout()

plt.savefig(output_path, dpi=300)

plt.close()

  

# 6.2 Satisfaction des Écoles (Algorithme de Boston)

def plot_school_satisfaction_boston(assignments, school_priorities, schools, output_path):

satisfaction = {'Priorités Remplies': 0, 'Priorités Non Remplies': 0}

for school, assigned_students in assignments.items():

for student in assigned_students:

# Vérifier si l'élève est parmi les préférés dans les limites de capacité

# Puisque chaque école a une capacité de 4, vérifier si l'élève est parmi les 4 premiers

if school_priorities[school].index(student) < schools[school]:

satisfaction['Priorités Remplies'] += 1

else:

satisfaction['Priorités Non Remplies'] += 1

labels = list(satisfaction.keys())

sizes = list(satisfaction.values())

colors = ['#a8dadc','#f1faee']

plt.figure(figsize=(8,8))

plt.pie(sizes, labels=labels, colors=colors, autopct='%1.1f%%', startangle=140)

plt.title('Satisfaction des Écoles (Algorithme de Boston)', fontsize=18)

plt.axis('equal')

plt.tight_layout()

plt.savefig(output_path, dpi=300)

plt.close()

  

# 6.3 Tableau des Étapes de l'Algorithme d'Acceptation Différée

def plot_algorithm_steps_da(steps, output_path):

df_steps = pd.DataFrame(steps)

plt.figure(figsize=(12,6))

ax = plt.gca()

ax.axis('off')

table = ax.table(cellText=df_steps.values, colLabels=df_steps.columns, cellLoc='center', loc='center', colColours=["#ffb6b9"]*len(df_steps.columns))

table.auto_set_font_size(False)

table.set_fontsize(12)

table.scale(1, 2)

plt.title("Étapes de l'Algorithme d'Acceptation Différée", fontweight='bold', fontsize=16)

plt.tight_layout()

plt.savefig(output_path, dpi=300)

plt.close()

  

# 6.4 Affectation Finale (Algorithme d'Acceptation Différée)

def plot_final_assignment_da(assignments, output_path):

candidats = list(assignments.keys())

etablissements_attribues = list(assignments.values())

plt.figure(figsize=(14,8))

sns.scatterplot(x=candidats, y=etablissements_attribues, s=200, color='skyblue')

for i, candidat in enumerate(candidats):

plt.text(candidat, etablissements_attribues[i], assignments[candidat], horizontalalignment='center', verticalalignment='bottom', fontsize=12)

plt.title('Affectation Finale (Algorithme d\'Acceptation Différée)', fontsize=18)

plt.xlabel('Candidats', fontsize=14)

plt.ylabel('Établissements', fontsize=14)

plt.xticks(rotation=45, fontsize=12)

plt.yticks(fontsize=12)

plt.tight_layout()

plt.savefig(output_path, dpi=300)

plt.close()

  

# 6.5 Légende des Propriétés des Algorithmes

def create_legend(output_path):

img_width, img_height = 1000, 400

img = Image.new('RGB', (img_width, img_height), color='white')

draw = ImageDraw.Draw(img)

font = get_font(16)

texte = """

Légende des Propriétés des Algorithmes d'Affectation:

  

• **Efficacité**:

Capacité de l'algorithme à maximiser la satisfaction des élèves en respectant leurs préférences.

  

• **Équité**:

Capacité de l'algorithme à répartir les places disponibles de manière juste, en respectant les priorités des écoles.

  

• **Non-manipulabilité**:

Capacité de l'algorithme à empêcher les élèves de manipuler leurs préférences pour obtenir de meilleures affectations.

  

• **Stabilité (pour l'Algorithme d'Acceptation Différée)**:

Garantie qu'aucune paire élève-école ne préférerait être avec quelqu'un d'autre.

  

• **Limitations**:

Contraintes inhérentes à l'algorithme, telles que l'incapacité à satisfaire simultanément efficacité, équité et non-manipulabilité.

"""

draw.multiline_text((20,20), texte, fill='black', font=font)

img.save(output_path)

  

# ================================

# 7. Génération des Graphiques

# ================================

  

# Génération des graphiques pour l'Algorithme de Boston

plot_student_satisfaction_boston(final_assignment_boston, student_prefs, os.path.join(output_dir, 'satisfaction_candidats_boston.png'))

plot_school_satisfaction_boston(final_assignment_boston, school_priorities, schools, os.path.join(output_dir, 'satisfaction_etablissements_boston.png'))

  

# Génération des graphiques pour l'Algorithme d'Acceptation Différée

# Définition des étapes de l'algorithme (exemple simplifié)

steps_da = [

{'Étape': '0', 'Action': 'Tous les élèves soumettent leurs listes de vœux.'},

{'Étape': '1', 'Action': 'Chaque élève propose à son premier choix.'},

{'Étape': '1', 'Action': 'Les écoles acceptent temporairement les propositions selon les priorités.'},

{'Étape': '2', 'Action': 'Les élèves rejetés proposent à leur deuxième choix.'},

{'Étape': '2', 'Action': 'Les écoles reconsidèrent les propositions et réaffectent temporairement.'},

{'Étape': '3', 'Action': 'Les élèves encore rejetés proposent à leur troisième choix.'},

{'Étape': '3', 'Action': 'Les écoles finalisent les affectations.'}

]

  

plot_algorithm_steps_da(steps_da, os.path.join(output_dir, 'algorithme_etapes_da.png'))

plot_final_assignment_da(final_assignment_da, os.path.join(output_dir, 'affectation_finale_da.png'))

  

# Génération de la légende

create_legend(os.path.join(output_dir, 'legende.png'))

  

# ================================

# 8. Vérification des Images Générées

# ================================

  

# Liste des sections attendues

sections = {

'Satisfaction des Élèves (Boston)': 'satisfaction_candidats_boston.png',

'Satisfaction des Écoles (Boston)': 'satisfaction_etablissements_boston.png',

'Étapes de l\'Algorithme d\'Acceptation Différée': 'algorithme_etapes_da.png',

'Affectation Finale (DA)': 'affectation_finale_da.png',

'Légende': 'legende.png'

}

  

# Vérifier si toutes les images ont été générées

all_generated = True

for section, filename in sections.items():

filepath = os.path.join(output_dir, filename)

if not os.path.exists(filepath):

print(f"Image manquante pour la section: {section}")

all_generated = False

  

if all_generated:

print(f"Toutes les images ont été générées avec succès dans le dossier '{output_dir}'.")

else:

print(f"Certaines images manquent. Veuillez vérifier les erreurs ci-dessus.")

```