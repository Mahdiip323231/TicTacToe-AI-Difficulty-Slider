import tkinter as tk
from tkinter import messagebox
import random
import math

def check_winner(board, player):
    """بررسی برنده شدن یک بازیکن"""
    win_patterns = [
        (0,1,2), (3,4,5), (6,7,8),
        (0,3,6), (1,4,7), (2,5,8),
        (0,4,8), (2,4,6)
    ]
    for a,b,c in win_patterns:
        if board[a] == player and board[b] == player and board[c] == player:
            return True
    return False

def is_full(board):
    """بررسی پر شدن صفحه"""
    return all(cell != ' ' for cell in board)

def get_available_moves(board):
    """لیست خانه‌های خالی"""
    return [i for i, cell in enumerate(board) if cell == ' ']

def minimax(board, depth, is_maximizing, ai_player, human_player):
    """الگوریتم مینی‌مکس برای انتخاب بهترین حرکت"""
    if check_winner(board, ai_player):
        return 1
    if check_winner(board, human_player):
        return -1
    if is_full(board):
        return 0

    if is_maximizing:
        best = -math.inf
        for move in get_available_moves(board):
            board[move] = ai_player
            score = minimax(board, depth+1, False, ai_player, human_player)
            board[move] = ' '
            best = max(best, score)
        return best
    else:
        best = math.inf
        for move in get_available_moves(board):
            board[move] = human_player
            score = minimax(board, depth+1, True, ai_player, human_player)
            board[move] = ' '
            best = min(best, score)
        return best

def best_move(board, ai_player, human_player):
    """بهترین حرکت ممکن برای هوش مصنوعی (سطح هوشمند 100%)"""
    best_score = -math.inf
    move = None
    for m in get_available_moves(board):
        board[m] = ai_player
        score = minimax(board, 0, False, ai_player, human_player)
        board[m] = ' '
        if score > best_score:
            best_score = score
            move = m
    return move

def random_move(board):
    """حرکت کاملاً تصادفی"""
    moves = get_available_moves(board)
    return random.choice(moves) if moves else None

def get_ai_move_with_difficulty(board, ai_player, human_player, difficulty):
    """
    ترکیب حرکت هوشمند و تصادفی بر اساس سطح دشواری (0 تا 100)
    difficulty=100 -> همیشه بهترین حرکت
    difficulty=0   -> همیشه حرکت تصادفی
    """
    if random.randint(0, 100) < difficulty:
        return best_move(board, ai_player, human_player)
    else:
        return random_move(board)

class TicTacToeGUI:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("🎮 بازی دوز | هوش مصنوعی قابل تنظیم")
        self.window.resizable(False, False)
        self.window.configure(bg="#1e272e")

        self.board = [' '] * 9
        self.current_player = 'X'   
        self.human = 'X'
        self.ai = 'O'
        self.game_over = False
        self.human_score = 0
        self.ai_score = 0
        self.difficulty = 50   

        self.create_widgets()
        self.update_turn_display()

    def create_widgets(self):
        """ایجاد تمام اجزای گرافیکی با چیدمان زیبا"""
        top_frame = tk.Frame(self.window, bg="#1e272e")
        top_frame.pack(pady=15)

        self.score_label = tk.Label(top_frame, text="👤 انسان: 0     🤖 کامپیوتر: 0",
                                    font=("Segoe UI", 14, "bold"),
                                    fg="#ffffff", bg="#1e272e")
        self.score_label.grid(row=0, column=0, columnspan=2, pady=5)

        diff_frame = tk.Frame(top_frame, bg="#1e272e")
        diff_frame.grid(row=1, column=0, columnspan=2, pady=10)

        tk.Label(diff_frame, text="سختی:", font=("Segoe UI", 11),
                 fg="#dfe6e9", bg="#1e272e").pack(side=tk.LEFT, padx=5)

        self.diff_slider = tk.Scale(diff_frame, from_=0, to=100, orient=tk.HORIZONTAL,
                                    length=200, bg="#2d3436", fg="#ffffff",
                                    highlightthickness=0, troughcolor="#636e72",
                                    sliderrelief="flat", command=self.update_difficulty)
        self.diff_slider.set(self.difficulty)
        self.diff_slider.pack(side=tk.LEFT, padx=5)

        self.diff_value_label = tk.Label(diff_frame, text=f"{self.difficulty}%",
                                         font=("Segoe UI", 10, "bold"),
                                         fg="#fdcb6e", bg="#1e272e", width=5)
        self.diff_value_label.pack(side=tk.LEFT, padx=5)

        self.board_frame = tk.Frame(self.window, bg="#1e272e")
        self.board_frame.pack(pady=10)

        self.buttons = []
        for i in range(3):
            row = []
            for j in range(3):
                btn = tk.Button(self.board_frame, text=" ", font=("Segoe UI", 32, "bold"),
                                width=4, height=2,
                                bg="#2d3436", fg="#dfe6e9",
                                activebackground="#636e72", activeforeground="#ffffff",
                                bd=0, relief="flat", highlightthickness=0,
                                command=lambda r=i, c=j: self.on_click(r, c))
                btn.grid(row=i, column=j, padx=5, pady=5, sticky="nsew")
                row.append(btn)
            self.buttons.append(row)

        bottom_frame = tk.Frame(self.window, bg="#1e272e")
        bottom_frame.pack(pady=15)

        self.turn_label = tk.Label(bottom_frame, text="نوبت: شما (X)",
                                   font=("Segoe UI", 12, "bold"),
                                   fg="#74b9ff", bg="#1e272e")
        self.turn_label.grid(row=0, column=0, columnspan=2, pady=5)

        restart_btn = tk.Button(bottom_frame, text="🔄 شروع مجدد",
                                font=("Segoe UI", 11, "bold"),
                                bg="#0984e3", fg="white", bd=0,
                                activebackground="#74b9ff", activeforeground="white",
                                padx=15, pady=5, command=self.reset_game)
        restart_btn.grid(row=1, column=0, padx=10, pady=5)

        menu_btn = tk.Button(bottom_frame, text="🏠 منوی اصلی",
                             font=("Segoe UI", 11, "bold"),
                             bg="#d63031", fg="white", bd=0,
                             activebackground="#ff7675", activeforeground="white",
                             padx=15, pady=5, command=self.back_to_menu)
        menu_btn.grid(row=1, column=1, padx=10, pady=5)

    def update_difficulty(self, val):
        """به‌روزرسانی مقدار دشواری هنگام حرکت اسلایدر"""
        self.difficulty = int(float(val))
        self.diff_value_label.config(text=f"{self.difficulty}%")

    def on_click(self, row, col):
        """وقتی کاربر روی خانه‌ای کلیک می‌کند"""
        if self.game_over:
            return
        index = row * 3 + col
        if self.board[index] != ' ' or self.current_player != self.human:
            return

        self.make_move(index, self.human)

        if self.game_over:
            return

        self.current_player = self.ai
        self.update_turn_display()
        self.window.after(300, self.ai_move)  

    def ai_move(self):
        """انجام حرکت هوش مصنوعی با در نظر گرفتن سطح دشواری"""
        if self.game_over or self.current_player != self.ai:
            return

        move = get_ai_move_with_difficulty(self.board, self.ai, self.human, self.difficulty)
        if move is not None:
            self.make_move(move, self.ai)
        else:
            if is_full(self.board):
                self.end_game("draw")

    def make_move(self, index, player):
        """انجام حرکت روی صفحه و به‌روزرسانی گرافیک"""
        self.board[index] = player
        row, col = divmod(index, 3)
        self.buttons[row][col].config(text=player,
                                      fg="#e17055" if player == 'X' else "#00b894",
                                      state="disabled")

        if check_winner(self.board, player):
            if player == self.human:
                self.human_score += 1
                winner_text = "شما برنده شدید! 🎉"
                winner_color = "#00b894"
            else:
                self.ai_score += 1
                winner_text = "کامپیوتر برنده شد! 🤖"
                winner_color = "#e17055"
            self.update_scores()
            self.end_game(winner_text, winner_color)
            return

        if is_full(self.board):
            self.end_game("draw")
            return

        self.current_player = self.ai if player == self.human else self.human
        self.update_turn_display()

    def update_turn_display(self):
        """به‌روزرسانی متن نوبت با رنگ‌بندی خاص"""
        if self.game_over:
            return
        if self.current_player == self.human:
            self.turn_label.config(text="نوبت: شما (X) 😊", fg="#74b9ff")
        else:
            self.turn_label.config(text="نوبت: کامپیوتر (O) 🤖", fg="#e17055")

    def update_scores(self):
        """به‌روزرسانی نمایش امتیازات"""
        self.score_label.config(text=f"👤 انسان: {self.human_score}     🤖 کامپیوتر: {self.ai_score}")

    def end_game(self, result, color=None):
        """پایان بازی و نمایش نتیجه بدون مسدود کردن پنجره"""
        self.game_over = True
        for i in range(3):
            for j in range(3):
                self.buttons[i][j].config(state="disabled")

        if result == "draw":
            message = "🤝 بازی مساوی شد!"
            color = "#fdcb6e"
        else:
            message = result

        self.turn_label.config(text=message, fg=color if color else "#ffffff")

    def reset_game(self):
        """شروع مجدد بازی (بدون تغییر امتیازات)"""
        self.board = [' '] * 9
        self.current_player = self.human
        self.game_over = False

        for i in range(3):
            for j in range(3):
                self.buttons[i][j].config(text=" ", state="normal", fg="#dfe6e9")

        self.update_turn_display()

    def back_to_menu(self):
        """بازنشانی کامل و بازگشت به حالت اولیه (برای سادگی، همان ریست)"""
        self.human_score = 0
        self.ai_score = 0
        self.update_scores()
        self.reset_game()

    def run(self):
        """اجرای حلقه اصلی برنامه"""
        self.window.mainloop()
        
if __name__ == "__main__":
    game = TicTacToeGUI()
    game.run()
