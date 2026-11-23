import re
from pathlib import Path

def analyze_text(filename):
    path = Path(filename)

    if not path.is_file():
        print(f"File not found: {filename}")
        return None

    try:
        text = path.read_text(encoding="utf-8")

        words = re.findall(r"\b\w+\b", text)
        sentences = re.findall(r"[.!?]+", text)

        word_count = len(words)
        sentence_count = len(sentences)

        average_length = (
            sum(len(word) for word in words) / word_count
            if word_count else 0
        )

        average_sentence = (
            word_count / sentence_count
            if sentence_count else 0
        )

        return {
            "words": word_count,
            "sentences": sentence_count,
            "average_word_length": average_length,
            "average_sentence_length": average_sentence
        }

    except OSError as error:
        print(f"Error: {error}")
        return None

def show_report(filename, stats):
    print(f"\n--- {filename} ---")
    print(f"Words: {stats['words']}")
    print(f"Sentences: {stats['sentences']}")
    print(
        f"Average word length: "
        f"{stats['average_word_length']:.2f}"
    )
    print(
        f"Average sentence length: "
        f"{stats['average_sentence_length']:.2f} words"
    )

def main():
    first = input("First text file: ").strip()
    second = input("Second text file: ").strip()

    result1 = analyze_text(first)
    result2 = analyze_text(second)

    if result1 is None or result2 is None:
        return

    show_report(first, result1)
    show_report(second, result2)

    print("\n--- Comparison ---")

    if result1["average_sentence_length"] > result2["average_sentence_length"]:
        print("First text has longer sentences.")
    elif result1["average_sentence_length"] < result2["average_sentence_length"]:
        print("Second text has longer sentences.")
    else:
        print("Both texts have equal average sentence length.")

    print(
        "Word count difference:",
        abs(result1["words"] - result2["words"])
    )

if __name__ == "__main__":
    main()
