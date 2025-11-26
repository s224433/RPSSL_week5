using System;
using Avalonia.Controls;
using Avalonia.Interactivity;

namespace RPSSL_week5
{
    public partial class MainWindow : Window
    {
        enum Move { Rock = 1, Paper, Scissors, Lizard, Spock }

        int score1 = 0;
        int score2 = 0;
        Random rnd = new Random();

        public MainWindow()
        {
            InitializeComponent();
            UpdateScoreText();
        }

        private void UpdateScoreText()
        {
            ScoreText.Text = "Player 1: " + score1 + "   Player 2: " + score2;
        }

        private void OnMoveClick(object? sender, RoutedEventArgs e)
        {
            // Stop if someone already won
            if (score1 == 3 || score2 == 3)
                return;

            Button b = (Button)sender;

            // Player 1 choice
            Move player1 = GetMoveFromButton(b);

            // Player 2 choice
            Move player2 = (Move)rnd.Next(1, 6);

            // Determine winner
            int result = RoundWinner(player1, player2);

            if (result == 1)
                score1++;
            else if (result == 2)
                score2++;

            UpdateScoreText();

            // Game end
            if (score1 == 3)
                WinnerText.Text = "Player 1 wins";
            else if (score2 == 3)
                WinnerText.Text = "Player 2 wins";
        }

        private Move GetMoveFromButton(Button b)
        {
            if (b.Name == "RockBtn") return Move.Rock;
            if (b.Name == "PaperBtn") return Move.Paper;
            if (b.Name == "ScissorsBtn") return Move.Scissors;
            if (b.Name == "LizardBtn") return Move.Lizard;
            return Move.Spock;
        }

        private int RoundWinner(Move a, Move b)
        {
            if (a == b)
                return 0;

            if ((a == Move.Rock     && (b == Move.Scissors || b == Move.Lizard)) ||
                (a == Move.Paper    && (b == Move.Rock     || b == Move.Spock)) ||
                (a == Move.Scissors && (b == Move.Paper    || b == Move.Lizard)) ||
                (a == Move.Lizard   && (b == Move.Spock    || b == Move.Paper)) ||
                (a == Move.Spock    && (b == Move.Rock     || b == Move.Scissors)))
            {
                return 1;
            }

            return 2;
        }
    }
}
