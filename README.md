# Digit Recognizer: MNIST CNN with a Live Drawing Canvas

A convolutional neural network trained on MNIST (handwritten digits 0-9), served through a small Flask web app.
Draw a digit in the browser and the model predicts it live, with a confidence bar for every digit.

## Files (keep them all in the same folder)

```
digit_recognizer/
├── numerical_pattern_predictor.ipynb   # your Week 1 notebook (unchanged)
├── model.py          # the CNN
├── preprocess.py     # turns a canvas drawing into a 28x28 MNIST-style image
├── train.py          # trains the model, saves weights and training curves
├── evaluate.py       # test accuracy, confusion matrix, misclassified digits
├── app.py            # Flask backend with a /predict endpoint
└── index.html        # the drawing canvas page
```

Running the scripts creates these extra items: `data/` (MNIST download), `digit_cnn.pth` (trained weights),
`history.json`, `results.json` and an `images/` folder with the plots.

## Steps to run

Open a terminal in the folder (in VS Code: Terminal > New Terminal). Run each command on its own line and wait for it to finish.

**1. Install the libraries** (skip if you already did)
```
pip install torch torchvision flask pillow numpy matplotlib
```

**2. Train the model** (the first run downloads MNIST, so you need internet)
```
python train.py
```
It trains for 10 epochs and prints the loss and accuracy after each one. On a normal laptop CPU expect roughly 5 to 15 minutes.
This creates `digit_cnn.pth` and `images/training_curves.png`.

**3. Evaluate on the test set**
```
python evaluate.py
```
Prints the test accuracy and per-digit precision and recall, and creates `images/confusion_matrix.png` and `images/misclassified.png`.

**4. Start the web app**
```
python app.py
```
Open **http://127.0.0.1:5000** in your browser and draw a digit. Press `Ctrl + C` in the terminal to stop the app.

If `python` is not recognized, use `py` instead (for example `py train.py`).

## How it works

1. **Model:** a CNN with 2 convolution blocks (Conv, BatchNorm, ReLU, MaxPool), then dropout and 2 fully connected layers.
2. **Training:** 10 epochs, Adam optimizer (learning rate 0.001), batch size 128, cross-entropy loss.
3. **Data split:** 54,000 train / 6,000 validation / 10,000 test. The test set is only used once, at the end.
4. **Augmentation:** random rotation, shift and zoom on the training images, so the model copes with messy hand-drawn digits.
5. **Preprocessing for the canvas:** the drawing is cropped, scaled so its longest side is 20 pixels, centred in a 28x28 frame by its
   centre of mass, and normalised exactly like the training data. This matches the MNIST format and is the key to accurate live predictions.
6. **Web app:** Flask serves the page and a `/predict` endpoint. The canvas sends the drawing as base64, and the server returns
   the digit, the confidence and all 10 probabilities.

## Results

Fill these in from your own run (the numbers will differ a little each time):

| Metric | Value |
|--------|-------|
| Test accuracy | (from `python evaluate.py`) |
| Best validation accuracy | (from the last lines of `python train.py`) |
| Parameters | (printed at the start of `python train.py`) |

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `digit_cnn.pth not found` | Run `python train.py` first and let it finish. |
| MNIST download fails or hangs | Check your internet, or try another network such as a phone hotspot, then run `python train.py` again. |
| `ModuleNotFoundError` | Run the pip install command above in the same terminal. |
| Port 5000 already in use | Close any other running app, or change `port=5000` in `app.py` to `5001` and open that address. |
| Predictions look wrong | Draw larger, in the middle of the box, with the digit upright. Click Clear between digits. |
| Page says it can't reach the server | `app.py` has stopped. Start it again with `python app.py`. |
