# Лабораторная Работа №4: Структуры Данных 
Задание: Словарь.
Дано слово и словарь. Необходимо найти все слова из словаря, которые можно составить из букв данного слова, и вывести их в порядке уменьшения длины.
В качестве словаря можно использовать любой текстовый файл с любыми словами, но желательно использовать словарь со словами русского языка.
Ограничение времени на решение 2 с.
Допустимо использовать больше времени (до 1 мин) на этап инициализации или обработки словаря. Но после этого этапа время на обработку нового слова не более 2 с.
Список слов (словарь) можно взять тут https://github.com/Harrix/Russian-Nouns/releases
Код протестирован на MS Visual Studio, кодировка только UTF-8!
Для ANSI 1251 код будет проще
## Реализация:
### Листинг программы:
``` python
import time
from collections import defaultdict, Counter

class WordFinder:
    def __init__(self, dictionary_file: str):
        """Инициализация с загрузкой словаря"""
        print("Автор: Анаприенко Даниил Сергеевич")
        print("Группа: АИСа-о24\n")
        
        self.start_time = time.time()
        self.word_dict = self.load_dictionary(dictionary_file)
        self.prepare_dictionary()
        print(f"Словарь загружен за {time.time() - self.start_time:.2f} сек")
    
    def load_dictionary(self, filename: str) -> list:
        """Загрузка словаря из файла"""
        with open(filename, 'r', encoding='utf-8') as f:
            return [line.strip().lower() for line in f if line.strip()]
    
    def prepare_dictionary(self):
        """Подготовка словаря для быстрого поиска"""
        self.words_by_length = defaultdict(list)
        self.word_signatures = {}
        
        for word in self.word_dict:
            length = len(word)
            self.words_by_length[length].append(word)
            self.word_signatures[word] = Counter(word)
        
        # Сортируем слова по длине (от большего к меньшему)
        self.sorted_lengths = sorted(self.words_by_length.keys(), reverse=True)
    
    def find_words(self, input_word: str) -> list:
        """Поиск слов, которые можно составить из букв входного слова"""
        start_time = time.time()
        input_word = input_word.lower()
        input_counter = Counter(input_word)
        result = []
        
        for length in self.sorted_lengths:
            if length > len(input_word):
                continue
                
            for word in self.words_by_length[length]:
                word_counter = self.word_signatures[word]
                if all(word_counter[char] <= input_counter[char] for char in word_counter):
                    result.append(word)
        
        print(f"Поиск выполнен за {time.time() - start_time:.4f} сек")
        return result

def main():
    # Пример использования
    print("Программа для поиска слов из букв заданного слова")
    print("Автор: Анаприенко Даниил Сергеевич")
    print("Группа: АИСа-о24\n")
    
    word_finder = WordFinder('russian_nouns.txt')  # Файл словаря
    
    while True:
        user_word = input("\nВведите слово (или 'exit' для выхода): ")
        if user_word.lower() == 'exit':
            break
            
        found_words = word_finder.find_words(user_word)
        print(f"\nНайдено {len(found_words)} слов:")
        for word in found_words:
            print(word)

if __name__ == "__main__":
    main()
```
# Результат выполнения программы:
![image](https://github.com/user-attachments/assets/b4412926-a176-47a2-9be4-1d12b5e973b7)
