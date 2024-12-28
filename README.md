# Maximize-Student-Selection

In a school playground, two lines of students are standing side by side - one line is made up of boys and the other of girls. Your task is to select some boys (or none) and some girls (or none) from these lines and arrange them in such a way that their heights form a strictly increasing order.
However, there are special rules you must follow:

No boy can stand in front of another boy who was originally ahead of him in the line.
No girl can stand in front of another girl who was originally ahead of her in the line.
The challenge is to select the maximum number of students you can while maintaining the original relative order of the boys and girls in their respective lines. How many students can you arrange while following these rules?

def max_students(i, j, last_height, boys, girls, dp):
    if i == len(boys) and j == len(girls):
        return 0
    if dp[i][j][last_height] != -1:
        return dp[i][j][last_height]

    result = 0

    # Include the current boy if possible
    if i < len(boys) and boys[i] > last_height:
        result = max(result, 1 + max_students(i + 1, j, boys[i], boys, girls, dp))

    # Include the current girl if possible
    if j < len(girls) and girls[j] > last_height:
        result = max(result, 1 + max_students(i, j + 1, girls[j], boys, girls, dp))

    # Skip the current boy
    if i < len(boys):
        result = max(result, max_students(i + 1, j, last_height, boys, girls, dp))

    # Skip the current girl
    if j < len(girls):
        result = max(result, max_students(i, j + 1, last_height, boys, girls, dp))

    dp[i][j][last_height] = result
    return result

def main():
    import sys
    input = sys.stdin.read
    data = input().splitlines()

    # Read input
    N, M = map(int, data[0].split())
    boys = list(map(int, data[1].split()))
    girls = list(map(int, data[2].split()))

    # Find maximum height
    max_height = max(max(boys, default=0), max(girls, default=0))

    # Initialize DP table with -1
    dp = [[[-1 for _ in range(max_height + 1)] for _ in range(M + 1)] for _ in range(N + 1)]

    # Solve the problem
    result = max_students(0, 0, 0, boys, girls, dp)

    # Print the result
    print(result)

if __name__ == "__main__":
    main()
