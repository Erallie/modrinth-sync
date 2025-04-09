# Modrinth Sync

[![Our Discord](https://img.shields.io/discord/1102582171207741480?style=for-the-badge&logo=discord&logoColor=ffffff&label=Our%20Discord&color=5865F2)](https://discord.gozarproductions.com)
[![Our Other Projects](https://img.shields.io/badge/Our%20Other%20Projects-%E2%9D%A4-563294?style=for-the-badge&logo=data%3Aimage%2Fwebp%3Bbase64%2CUklGRu4DAABXRUJQVlA4WAoAAAAQAAAAHwAAHwAAQUxQSGABAAABgFtbm5volyZTA%2BtibzK2H0w5sDkmhe3GmxrwxGg0839r%2FvkkOogIBW7bKB0c4%2BARYihzIqfd6dfO%2B%2B3XtHsq4jJhlIvcDRcgNB%2FeieQETorBHgghRtUYqwDs%2B4U4IpcvUB%2BVUPSK54uEnTwsUJoar2DeMpzLxQpeG5DH8lxyyfLivVYAwPBbkWdOBg3qFlqiLy679iHy9UDKMZRXmYxpCcusayTHG01K%2FEtatYWuj7oI9hL4BxsxVwhoP2mlAJJ%2BuuAflc6%2BEUCQTCX9EV87xBR2H75NxLZSpWiwzqdIm7ZO7uB3oEgZKbD9Nt3EmHweEPH1t1GNsZUbKeisiwjyTm5fA3SO1yCrADZXrV2PZQJPL1tjN4%2BxUL9ie1mJobzOnDwSx6ILiF%2FW%2BTUR4tcHx0UaV75JXC1a4g6Ky5dLcTSuy9q4HhTieF64Hy1A3GHB8gLLK2e92feuqnbfPK8IVlA4IGgCAACwDQCdASogACAAPk0cjEQioaEb%2BqwAKATEtgBOl7v9V3sHcA2wG4A3gD0APLP9jX9n%2F2jmqv5AZRh7J%2BN2fOx22iE%2F4TUsecFmY%2BSf1r%2BAP%2BTfzT%2FXdIB7KX7MtdIGr1A8H0jmrrfZvqButwOaYcLWYNRq5QgAAP7%2F%2FmIMpiVNn67QXpM1rrDmRS8Nr%2F6dhD%2Bq5e%2BM%2BAtUP1%2FxOj85Ol5y3ebjz%2BpHoOf%2FWW8a%2F2ojUaKVDkVqof%2Bv4f0f6ud8i58wusz%2Fyrj%2F%2BwnM3q0769dvK%2F%2BQe04xL49tkb9t6ylCqqezZtZGuGLJ%2F5iUrPqdYc%2F8VbYZfP%2FOpZP%2F4X4q%2BqS4gPOxzdINOe5PGv%2F0TS%2FJRf4LlFrFkrWtxlS8n40grV%2BKUu%2FiwzdQzImvwH81FxL1bZyTSsrYwMku1Pk9StTtWNjSR8ZWEYBH9eTn%2FvBERii5XaWOPJ%2FFVXtVQGbv%2BFRW5jbo9tfFDu%2BDHHf8LbgUd%2F8W8Id1AehBtRNsLQWbADmvF1QJU8x5tw%2FtTUwIoSaa%2F2jkcvyVHkAsb2qoIh1KF1pPdae%2BZaqjydy6nUa9agjrDk1G4pMhEUhH%2BV%2FIUe49MjhR%2FuxyFmwQ8dDogMyQ%2BdcSBa56Lwt1wyJ%2F22%2F5O98r6q6wiM63HyaYONd36W7br%2F0%2F6y2DZ3irAddj%2FRxntvr%2FbbChSYXAfEbO%2FD0G%2FFbMFqTHypodt9T6dAx%2BUjJYfHzFf%2FM3Ec%2FAtwbjc2gka6urN1MlSLb2VTS9Q5r8fkDzxZz6vu1OYUPUB1UFMIhYGvMATbxxoTmVhvpovzAc%2F8nbOjw3wAAA)](https://github.com/Erallie)
[![Donate](https://img.shields.io/badge/Donate-%24-563294?style=for-the-badge&logo=paypal&color=rgb(0%2C%2048%2C%20135))](https://www.paypal.com/donate/?hosted_button_id=PHHGM83BQZ8MA)

---

A set of workflows that allow your plugin releases to be automatically pushed to modrinth.

These workflows sync your modrinth releases when you do the following on GitHub:
- Publish a new release
    - It uses your attached `.jar` binary as the primary file.
    - It gets the version from your GitHub tag.
    - It gets the release name from your GitHub release name.
- Edit a release
    - Can edit all synced fields except the primary file.
- Delete a release
    - It deletes the release from your modrinth project
- Edit your `README.md`
    - It uses your `README.md` as your project description.

# Setup
1. Create a file named `modrinth-sync.yml` in the folder `.github/workflows` in your GitHub repository.
2. Copy and paste the following into the file:
    ```yml
    name: Modrinth Sync

    on:
        release:
            types: [published, edited, deleted]
        push:
            branches:
                - main

    jobs:
        publish-release:
            if: github.event_name == 'release' && github.event.action == 'published'
            uses: Erallie/modrinth-sync/.github/workflows/publish-release.yml@main
            with:
                project_id: "XXXXXXXX"
                game_versions: '["1.21", "1.21.1", "1.21.2", "1.21.3", "1.21.4"]'
                loaders: '["spigot", "paper", "purpur"]'
                dependencies: >-
                    [
                        { "version": "*", "project_id": "XXXXXXXX", "dependency_type": "required" },
                        { "version": "*", "project_id": "XXXXXXXX", "dependency_type": "required" }
                    ]
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
        
        edit-release:
            if: github.event_name == 'release' && github.event.action == 'edited'
            uses: Erallie/modrinth-sync/.github/workflows/edit-release.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
        
        delete-release:
            if: github.event_name == 'release' && github.event.action == 'deleted'
            uses: Erallie/modrinth-sync/.github/workflows/delete-release.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}

        edit-project-description:
            if: github.event_name == 'push'
            uses: Erallie/modrinth-sync/.github/workflows/edit-project-description.yml@main
            with:
                project_id: "XXXXXXXX"
            secrets:
                MODRINTH_TOKEN: ${{ secrets.MODRINTH_TOKEN }}
    ```
3. Replace the `project_id` in all actions with your Modrinth project's id.
4. Replace `game_versions`, `loaders`, and `dependencies` under `publish-release` with the correct data for your Modrinth project.
5. Commit the changes.