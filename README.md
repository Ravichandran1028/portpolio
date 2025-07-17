
stock_prices = {
    "AAPL": 180,
    "TSLA": 250,
    "GOOGL": 2800,
    "AMZN": 3500,
    "MSFT": 330
}


portfolio = {}


print("Enter your stock holdings (symbol and quantity). Type 'done' to finish.")
while True:
    symbol = input("Stock symbol (e.g., AAPL): ").upper()
    if symbol == 'DONE':
        break
    if symbol not in stock_prices:
        print(f"Stock symbol '{symbol}' not found in price list.")
        continue
    try:
        quantity = int(input(f"Quantity of {symbol}: "))
        if quantity < 0:
            raise ValueError
        portfolio[symbol] = portfolio.get(symbol, 0) + quantity
    except ValueError:
        print("Invalid quantity. Please enter a positive integer.")


total_investment = 0
print("\nYour Portfolio:")
for symbol, quantity in portfolio.items():
    price = stock_prices[symbol]
    value = quantity * price
    total_investment += value
    print(f"{symbol}: {quantity} shares × ${price} = ${value}")

print(f"\nTotal Investment Value: ${total_investment}")


save = input("Do you want to save the result to a file? (yes/no): ").lower()
if save == 'yes':
    file_type = input("Choose file type: txt or csv: ").lower()
    if file_type == 'txt':
        with open("portfolio_summary.txt", "w") as file:
            for symbol, quantity in portfolio.items():
                file.write(f"{symbol}: {quantity} shares × ${stock_prices[symbol]} = ${quantity * stock_prices[symbol]}\n")
            file.write(f"\nTotal Investment Value: ${total_investment}")
        print("Portfolio saved to portfolio_summary.txt")
    elif file_type == 'csv':
        import csv
        with open("portfolio_summary.csv", "w", newline='') as file:
            writer = csv.writer(file)
            writer.writerow(["Stock", "Quantity", "Price", "Value"])
            for symbol, quantity in portfolio.items():
                writer.writerow([symbol, quantity, stock_prices[symbol], quantity * stock_prices[symbol]])
            writer.writerow(["", "", "Total", total_investment])
        print("Portfolio saved to portfolio_summary.csv")
    else:
        print("Invalid file type. Skipping save.")
