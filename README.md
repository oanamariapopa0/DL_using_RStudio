# DL_using_RStudio <3
Simple CNN using a synthetic dataset trained by diagnostic features.

# Load the required package
install.packages("neuralnet")
library(neuralnet)

# Create a synthetic dataset

set.seed(123)
# For reproducibility
n <- 100
features <- data.frame(Feature1 = rnorm(n), Feature2 = rnorm(n))
target <- ifelse(features$Feature1 + features$Feature2 > 0, 1, 0)
synthetic_data <- cbind(features, Diagnosis = target)
# Convert 'Diagnosis' to binary (0 and 1)
synthetic_data$Diagnosis <- ifelse(synthetic_data$Diagnosis == 1, "Diseased", "Healthy")
# Define the neural network formula
nn_formula <- as.formula("Diagnosis ~ Feature1 + Feature2")

# Train the neural network
nn <- neuralnet(nn_formula, data = synthetic_data, hidden = c(3), linear.output = FALSE)
# Make predictions on the training data
predictions <- compute(nn, synthetic_data[, c("Feature1", "Feature2")])
predicted_diagnosis <- ifelse(predictions$net.result > 0.5, "Diseased", "Healthy")

# Compare predictions with actual diagnosis
comparison <- data.frame(Actual = synthetic_data$Diagnosis, Predicted = predicted_diagnosis)
print(comparison)
summary(comparison)
plot(nn)
comparison <- cbind(synthetic_data, Predicted = predicted_diagnosis)

#plotting
library(ggplot2)
install.packages("ggplot2")

plot(x = synthetic_data$Feature1, y = synthetic_data$Feature2, col = ifelse(synthetic_data$Diagnosis == "Healthy", "blue", "red"), pch = ifelse(comparison$Predicted == "Healthy", 16, 17), main = "Actual Diagnosis vs. Predicted Diagnosis", xlab = "Feature1", ylab = "Feature2", xlim = c(min(synthetic_data$Feature1), max(synthetic_data$Feature1)), ylim = c(min(synthetic_data$Feature2), max(synthetic_data$Feature2)))
legend("topright", legend = c("Healthy", "Diseased"), col = c("blue", "red"), pch = c(16, 17), title = "Diagnosis", bg = "white", cex = 0.8)
title(main = "Actual Diagnosis vs. Predicted Diagnosis", xlab = "Feature1", ylab = "Feature2")
