/* Required Imports */
import java.util.Random;
import javafx.application.Application;
import javafx.application.Platform;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.layout.GridPane;
import javafx.scene.control.Alert;

public class project extends Application{
    int board[][]={{0,0,0,0},{0,0,0,0},{0,0,0,0},{0,0,0,0}};   
    
    /* Method to generate an random number */
    void generate(){
        if(checkGameOver()) return;
        int flag=0,arr[]={2,2,2,2,4,2,2,2,4,2};
        Random r=new Random();
        for(int [] x:board){
            for(int y:x){
                if(y==0){
                    flag=1;
                    break;
                }
            }
            if(flag==1) break;
        }
        if(flag==0 ){
            checkGameOver();
            return;
        }
        while(true)
        {
            int i=r.nextInt(4);
            int j=r.nextInt(4);
            if(board[i][j]==0){
                int k=r.nextInt(10);
                board[i][j]=arr[k];
                break;}
        }

    }
    /*Method to check the availability of empty cell 
      or possibility of any move */
    boolean isOver(){
        for(int i=0;i<4;i++){
            for(int j=0;j<4;j++){
                if(board[i][j]==0) return false;
                if(j<3 && (board[i][j]==board[i][j+1] || board[j][i]==board[j+1][i])) return false; 
            }
        }
        return true;
    }
    /* Method to check GameOver */
    boolean checkGameOver() {
        if(isOver()){
            Alert alert=new Alert(Alert.AlertType.INFORMATION);
            alert.setTitle("Md Umer's 2048");
            alert.setHeaderText("No more moves Possible");
            alert.setContentText("Game Over!");
            alert.showAndWait();
            Platform.exit();    
        }
        return false;
    }

    public void start(Stage PrimaryStage){
            generate();
            GridPane gpane = new GridPane();
            showGrid(gpane);
            Scene scene=new Scene(gpane,400,400);
            PrimaryStage.setScene(scene);
            PrimaryStage.setTitle("2048 By Md Umer S");
            PrimaryStage.show();
            scene.setOnKeyPressed(event->{
                boolean moved=false;
                switch(event.getCode()){
                    case UP -> moved=moveUp();
                    case DOWN -> moved=moveDown();
                    case RIGHT -> moved=moveRight();
                    case LEFT -> moved=moveLeft();
                }
                if(moved){ 
                   generate();  
                   showGrid(gpane);
                   checkGameOver();   
                }
                else{
                    checkGameOver();
                    generate();
                    showGrid(gpane);
                }   
            });
            PrimaryStage.setScene(scene);
            PrimaryStage.setTitle("2048 By Md Umer S");
            PrimaryStage.show();
    }
    /*For displaying grid */
    void showGrid(GridPane gpane){
        for(int row=0;row<4;row++){
            for(int col=0;col<4;col++){
                Label label;
                if(board[row][col]==0)
                    label=new Label(""); 
                else 
                    label=new Label(""+board[row][col]);
                
                label.setStyle(
                "-fx-background-color:"+getColor(board[row][col])+";"+
                "-fx-font-size:24px;"+
                "-fx-text-fill:black;"+
                "-fx-font-weight:bold;"+
                "-fx-border-color:black;"+
                "-fx-border-width:2;"+
                "-fx-border-radius: 5px;" +
                "-fx-alignment:center");
                label.setPrefSize(100,100);
                gpane.add(label,col,row);
            }
        }
    }
    /* Different colors for  different Values*/
    String getColor(int value){
        return switch (value) {
            case 2 -> "#EEE4DA";
            case 4 -> "#EDE0C8";
            case 8 -> "#F2B179";
            case 16 -> "#F59563";
            case 32 -> "#F67C5F";
            case 64 -> "#F65E3B";
            case 128 -> "#EDCF72";
            case 256 -> "#EDCC61";
            case 512 -> "#EDC850";
            case 1024 -> "#EDC53F";
            case 2048 -> "#EDC22E";
            default -> "#CDC1B4";
        };
    }
    /* Moving elemnts towards up */
    void upCheck(){
        for(int i=0;i<4;i++){ 
            for(int j=0;j<4;j++){
                if(board[j][i]!=0) continue;
                for(int k=j+1;k<4;k++){
                    if(board[k][i]!=0){
                        board[j][i]=board[k][i];
                        board[k][i]=0;break;
                    }
                }
            }
        }
    }
    /* Moving elemnts towards down */
    void downCheck(){
        for(int i=0;i<4;i++){
            for(int j=3;j>=0;j--){
                if(board[j][i]!=0) continue;
                for(int k=j-1;k>=0;k--){
                    if(board[k][i]!=0){
                        board[j][i]=board[k][i];
                        board[k][i]=0;break;
                    }
                }
            }
        }
    }
    /* Moving elemnts towards right */
    void rightCheck(){
        for(int i=0;i<4;i++){
            for(int j=3;j>=0;j--){
                if(board[i][j]!=0) continue;
                for(int k=j-1;k>=0;k--){
                    if(board[i][k]!=0){
                        board[i][j]=board[i][k];
                        board[i][k]=0;break;
                    }
                }
            }
        }
    }
    /* Moving elemnts towards left */
    void leftCheck(){
        for (int i=0;i<4;i++){
            for(int j=0;j<4;j++){
               if(board[i][j]!=0) continue;
               for(int k=j+1;k<4;k++){
                 if(board[i][k]!=0){
                    board[i][j]=board[i][k];
                    board[i][k]=0;break;    
                 }
               } 
            }
        }
    }
    /* Merging elements on UPclick */
    boolean moveUp(){
        boolean moved=false;
        upCheck();
        for(int i=0;i<4;i++){
            for(int j=0;j<3;j++){
                if(board[j][i]==board[j+1][i]){
                    board[j][i] = 2*board[j][i];
                    board[j+1][i]=0;
                    moved=true;
                }
            }
        }
        upCheck();
        return moved;
    }
    /* Merging elements on DOWNtclick */
    boolean moveDown(){
        boolean moved=false;
        downCheck();
        for(int i=0;i<4;i++){
            for(int j=3;j>0;j--){
                if(board[j][i]==board[j-1][i]){
                    board[j][i] = 2*board[j][i];
                    board[j-1][i] = 0; 
                    moved=true;
                } 
            }
        }
        downCheck();
        return moved;
    }
    /* Merging elements on Rightclick */
    boolean moveRight(){
        boolean moved=false;
        rightCheck();
        for(int i=0;i<4;i++){
            for(int j=3;j>0;j--){
                if(board[i][j]==board[i][j-1]){
                    board[i][j] = 2*board[i][j];
                    board[i][j-1]=0;
                    moved=true;
                }
            }
        }
        rightCheck();
        return moved;
    }
    /* Merging elements on LEFTclick */
    boolean moveLeft(){    
        boolean moved=false;
        leftCheck();
        for(int i=0;i<4;i++)
            {   
                for(int j=0;j<3;j++){
                    if(board[i][j]==board[i][j+1]){
                        board[i][j] = board[i][j]*2;
                        board[i][j+1] = 0;
                        moved=true;
                    }
                } 
            }
        leftCheck();
        return moved;
    }
    public static void main(String []args){
        launch(args);
    }
}
