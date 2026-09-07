# PyAV 18.1.0 abi3 wheel installs on Python 3.14 and carries libx264

ts: 2026-09-06T17:29:09Z
commit: none (traverse has no commits; working tree untracked)
session: https://claude.ai/code/session_011hdAYizVaMEkLF8FU5jzFU
status: verified
fact: On the Windows dev box with Python 3.14.2, `pip download av` resolves `av-18.1.0-cp311-abi3-win_amd64.whl`, and a scratch venv with that wheel encodes libx264 and decodes 40 frames at 20 fps without a system ffmpeg. Plan 1 Task 7's fixture generator depends on this.
basis: probe `enc.py` in the session scratchpad (t.mp4 mtime 2026-09-06 13:29:09 -0400) printed `av 18.1.0` / `rate 20.0 frames 40`; re-captured 2026-09-07T03:53:39Z: `"$SCRATCH/avv/Scripts/python" -c "import av; print(av.__version__, 'libx264' in av.codecs_available)"` printed `18.1.0 True`; `python -m pip index versions av` printed `av (18.1.0)`.
re-verify: python -m pip index versions av | head -1
