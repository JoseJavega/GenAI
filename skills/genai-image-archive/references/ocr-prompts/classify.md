# Clasificación de Tipo de Documento

Analiza esta imagen de documento genealógico y determina su tipo.

Responde ÚNICAMENTE con una de estas opciones (una palabra):

- `bautismo` — Partida o certificado de bautismo. Menciona "bauticé", "bautismo", "nació", "hijo legítimo de", padrinos, párroco oficiante.
- `matrimonio` — Acta o certificado de matrimonio. Menciona "contraer matrimonio", "casé", "desposé", "velaciones", novios, testigos.
- `defuncion` — Acta o certificado de defunción. Menciona "falleció", "enterré", "difunto", "cadáver", "viudo de", causa de muerte.
- `otro` — No es ninguno de los anteriores o no puedes determinarlo con ≥70% de confianza.

Reglas:
- Si el documento contiene múltiples registros (ej. libro parroquial con varias actas), clasifica por el primer registro completo visible.
- Si hay ambigüedad entre bautismo y defunción (ej. "Josefa, hija de... falleció..."), prioriza por el verbo principal.
- No añadas explicaciones, solo la palabra clave.
