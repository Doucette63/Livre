name: Vinted Bot

on:
  workflow_dispatch:
  schedule:
    - cron: '*/30 * * * *'   # relance toutes les 30 min (marge de sécurité contre les déclenchements ratés par GitHub)

jobs:
  run-bot:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    concurrency:
      group: vinted-bot
      cancel-in-progress: true

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.x"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install requests

      - name: Run Vinted Bot
        env:
          VINTED_URLS: "https://www.vinted.fr/catalog?search_text=il%20%C3%A9tait%20une%20fois%20un%20coeur%20bris%C3%A9&search_by_image_uuid=&search_by_image_id=&page=1&time=1788249354&catalog[]=2312,https://www.vinted.fr/catalog?search_text=fae%20de%20sel%20et%20de%20sang&search_by_image_uuid=&search_by_image_id=&page=1&time=1788249384&catalog[]=2312,https://www.vinted.fr/catalog?search_text=le%20peuple%20de%20l%27air&search_by_image_uuid=&search_by_image_id=&page=1&time=1788249407&catalog[]=2312"
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
          DISCORD_WEBHOOK_STATUS: ${{ secrets.DISCORD_WEBHOOK_STATUS }}
        run: python main.py

      - name: Commit seen.json
        run: |
          git config --global user.name "Vinted Bot"
          git config --global user.email "vinted-bot@example.com"
          git add seen.json
          git commit -m "Update seen.json [ci skip]" || echo "✅ Pas de nouvelles annonces à commit"
          git push
