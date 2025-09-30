import os
import datetime
import whisper
import srt
from tkinter import Tk, filedialog, Label, Button, messagebox, Scrollbar, RIGHT, Y, LEFT, BOTH, Frame

try:
    from pydub import AudioSegment
except ImportError:
    AudioSegment = None

def generate_srt_one_word(audio_path, output_dir):
    model = whisper.load_model("base")
    result = model.transcribe(audio_path, language="en", fp16=False, word_timestamps=True)

    words = []
    for seg in result.get("segments", []):
        words.extend(seg.get("words", []))

    if not words:
        messagebox.showerror("Fehler", "Keine Wörter mit Zeitstempel gefunden.")
        return False

    subs = []
    idx = 1

    for w in words:
        word_clean = ''.join(c for c in w["word"] if c.isalnum())
        if not word_clean:
            continue
        subs.append(srt.Subtitle(
            index=idx,
            start=datetime.timedelta(seconds=w["start"]),
            end=datetime.timedelta(seconds=w["end"]),
            content=word_clean
        ))
        idx += 1

    filename = os.path.basename(audio_path).rsplit(".", 1)[0]
    os.makedirs(output_dir, exist_ok=True)
    srt_path = os.path.join(output_dir, f"{filename}_1word_clean.srt")
    with open(srt_path, "w", encoding="utf-8") as f:
        f.write(srt.compose(subs))

    return srt_path

def main():
    root = Tk()
    root.title("🎯 1-Wort SRT Generator")
    root.geometry("700x300")
    root.resizable(False, False)

    audio_path_var = [None]

    def select_audio():
        fpath = filedialog.askopenfilename(
            title="Wähle eine Audiodatei (MP3, WAV, M4A)",
            filetypes=[("Audio files", "*.mp3 *.wav *.m4a")]
        )
        if fpath:
            audio_path_var[0] = fpath
            label_audio_path.config(text=f"Ausgewählte Datei:\n{fpath}")

    def generate_srt_clicked():
        if not audio_path_var[0]:
            messagebox.showwarning("Warnung", "Bitte wähle eine Audiodatei aus.")
            return

        output_directory = filedialog.askdirectory(title="Ordner zur Speicherung der SRT-Datei wählen")
        if not output_directory:
            return

        btn_generate.config(state="disabled")
        root.update()
        srt_file = generate_srt_one_word(audio_path_var[0], output_directory)
        btn_generate.config(state="normal")

        if srt_file:
            messagebox.showinfo("Fertig", f"SRT-Datei wurde gespeichert:\n{srt_file}")
        else:
            messagebox.showerror("Fehler", "Fehler beim Erstellen der SRT-Datei.")

    frame_audio = Frame(root)
    frame_audio.pack(padx=10, pady=10, fill='x')

    btn_select_audio = Button(frame_audio, text="Audio Datei auswählen", command=select_audio)
    btn_select_audio.pack(side=LEFT)

    label_audio_path = Label(frame_audio, text="Keine Datei ausgewählt", wraplength=600, justify="left")
    label_audio_path.pack(side=LEFT, padx=10)

    btn_generate = Button(root, text="SRT erstellen", command=generate_srt_clicked, height=2, width=20)
    btn_generate.pack(pady=20)

    root.mainloop()

if __name__ == "__main__":
    main()
