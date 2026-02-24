<img width="951" height="675" alt="image" src="https://github.com/user-attachments/assets/36236f49-beaf-46e2-b0f7-48e8aa48622f" />
<img width="922" height="143" alt="image" src="https://github.com/user-attachments/assets/ccf6f30e-03e1-4c9b-b3ea-be9482606581" />

```ts
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity('target_setup')
export class TargetSetupEntity {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  branch_id: string;

  @Column({ type: 'text' })
  years: string;

  @Column({ type: 'list', typeDef: '<text>' })
  questions_target: string[];

  @Column({ type: 'map', typeDef: '<text, int>' })
  score_map: Record<string, number>;

  @Column({ type: 'set', typeDef: '<uuid>' })
  unique_ids: string[];
}
```

```
@Entity('target_setup')
export class TargetSetupEntity {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  branch_id: string;

  @Column({ type: 'text' })
  years: string;

  @Column({ type: 'list', typeDef: '<text>' })
  questions_target: string[];

  @Column({ type: 'set', typeDef: '<uuid>' })
  unique_ids: string[];

  @Column({ type: 'map', typeDef: '<text, int>' })
  score_map: Record<string, number>;
}
```
