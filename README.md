# fractional_knapsack.py

def greedy_knapsack(values, weights, capacity):
    items = [(v/w, v, w) for v, w in zip(values, weights)]
    items.sort(reverse=True)
    total_value = 0
    knapsack = []

    for ratio, value, weight in items:
        if capacity >= weight:
            capacity -= weight
            total_value += value
            knapsack.append((value, weight))
        else:
            fraction = capacity / weight
            total_value += value * fraction
            knapsack.append((value * fraction, capacity))
            break
    return total_value, knapsack

def simulated_dp_knapsack(values, weights, capacity, scale=100):
    n = len(values)
    W = int(capacity * scale)
    weights_scaled = [int(w * scale) for w in weights]
    dp = [0] * (W + 1)

    for i in range(n):
        for w in range(W, weights_scaled[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights_scaled[i]] + values[i])

    return max(dp)  # Approximation

def main():
    print("\nFRACTIONAL KNAPSACK PROBLEM SOLVER")
    n = int(input("Enter number of items: "))
    values = list(map(float, input("Enter the values of items: ").split()))
    weights = list(map(float, input("Enter the weights of items: ").split()))
    capacity = float(input("Enter the capacity of the knapsack: "))

    print("\n--- Greedy Algorithm ---")
    total_val_greedy, selected = greedy_knapsack(values, weights, capacity)
    print(f"Total value: {total_val_greedy:.2f}")
    for val, wt in selected:
        print(f"Item: Value = {val:.2f}, Weight = {wt:.2f}")

    print("\n--- Simulated Dynamic Programming ---")
    total_val_dp = simulated_dp_knapsack(values, weights, capacity)
    print(f"Total value (approx.): {total_val_dp:.2f}")

if __name__ == '__main__':
    main()
