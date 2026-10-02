FROM ollama/ollama
RUN bash -c 'ollama serve & sleep 5 && ollama pull qwen3:14b && kill %1'