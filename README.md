from flask import Flask, render_template, request
import random

app = Flask(__name__)

wardrobe = {
    "tops": ["T-shirt", "Blouse", "Sweater", "Tank top", "Dress Shirt"],
    "bottoms": ["Jeans", "Skirt", "Shorts", "Dress Pants", "Leggings"],
    "shoes": ["Sneakers", "Heels", "Sandals", "Boots", "Loafers"],
    "accessories": ["Watch", "Necklace", "Hat", "Bracelet", "Sunglasses"]
}

occasions = {
    "Beach": {
        "tops": ["Tank top", "T-shirt"],
        "bottoms": ["Shorts", "Skirt", "Leggings"],
        "shoes": ["Sandals"],
        "accessories": ["Hat", "Sunglasses"]
    },
    "Office": {
        "tops": ["Blouse", "Dress Shirt", "Sweater"],
        "bottoms": ["Dress Pants", "Skirt"],
        "shoes": ["Loafers", "Heels"],
        "accessories": ["Watch", "Bracelet"]
    },
    "Party": {
        "tops": ["Blouse", "Dress Shirt"],
        "bottoms": ["Skirt", "Jeans"],
        "shoes": ["Heels", "Boots"],
        "accessories": ["Necklace", "Bracelet"]
    },
    "Hiking": {
        "tops": ["T-shirt", "Sweater"],
        "bottoms": ["Jeans", "Shorts", "Leggings"],
        "shoes": ["Sneakers", "Boots"],
        "accessories": ["Hat", "Sunglasses"]
    }
}

def score_outfit(occasion, choices):
    prefs = occasions[occasion]
    score = sum([choices[k] in prefs[k] for k in choices])
    return score

@app.route('/', methods=['GET', 'POST'])
def wardrobe_game():
    if request.method == 'POST':
        occasion = request.form['occasion']
        choices = {cat: request.form[cat] for cat in wardrobe}
        score = score_outfit(occasion, choices)
        feedback = "Awesome! Perfect choice!" if score == 4 else "Try again for a perfect match!"
        return render_template('result.html', score=score, feedback=feedback, occasion=occasion, choices=choices)
    else:
        occasion = random.choice(list(occasions.keys()))
        return render_template('game.html', wardrobe=wardrobe, occasion=occasion)

if __name__ == '__main__':
    app.run(debug=True)
